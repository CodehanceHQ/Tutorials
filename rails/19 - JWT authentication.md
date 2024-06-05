# Adding JWT Authentication for Devise User in Rails 7 API with GraphQL

```

# Adding JWT Authentication for Devise User in Rails 7 API with GraphQL

## Step 1: Add Devise JWT Gem
# Open your Gemfile and add the following gems:
gem 'devise-jwt', '~> 0.11.0'

# Install the gems
bundle install

# Questions:
# 1. What is the purpose of the `devise-jwt` gem in this setup?
# 2. What would you do if you encounter a version conflict during gem installation?

## Step 2: Create JwtDenylist Model
# Generate a model for JWT denylist
rails generate model JwtDenylist jti:string:index exp:datetime

# Questions:
# 1. Why do we need a `JwtDenylist` model?
# 2. What does the `rails generate model` command do?
# 3. Why is `jti:string:index` used in the model generation command?

## Step 3: Add JWT Secret to Credentials
# Run the following command to add JWT secret to your credentials

# run this in terminal and copy the output
rails secret

# run this in first cmd tab
# this will launch into vscode waiting for update
# IMPORTANT: close file after changes for --wait to be terminated
EDITOR="code --wait" bin/rails credentials:edit

# in vscode update to match below
secret_key_base: leave_this_as_was_generated
jwt_secret_key: enter_your_secret_key_copied_from_cmd

# note: after saving the file will only contain encrypted one liner, when
# decrypted then it shows more info.

# Questions:
# 1. Why is it important to keep the JWT secret key secure?
# 2. What is the purpose of the `rails secret` command?
# 3. How do you edit Rails credentials?
# 4. What is master.key where is it and when is it generated?
# 5. Why is it important for master.key to be in .gitignore?
# 6. How would fellow collaborative coders get master.key?
# 7. Is it safe to commit credentials.yml.enc and why?

## Step 4: Configure Devise for JWT
# Create an initializer for JWT configuration
# config/initializers/devise.rb
Devise.setup do |config|
  # ==> JWT configuration
  config.jwt do |jwt|
    jwt.secret = Rails.application.credentials.jwt_secret_key
    jwt.dispatch_requests = [
      ['POST', %r{^/login$}],
      ['POST', %r{^/graphql$}]
    ]
    jwt.revocation_requests = [
      ['DELETE', %r{^/logout$}]
    ]
    jwt.expiration_time = 1.day.to_i
  end
end

# Questions:
# 1. What does the `jwt.secret` configuration do?
# 2. Why do we define `dispatch_requests` and `revocation_requests`?
# 3. What is the significance of `jwt.expiration_time`?

## Step 5: Include JWT Middleware
# Add JWT middleware to the Warden configuration
# config/initializers/warden.rb

# generate config/initializers/warden.rb
touch config/initializers/warden.rb

# config/initializers/warden.rb
Warden::JWTAuth.configure do |config|
  # Secret key to encode and decode JWT tokens
  config.secret = Rails.application.credentials.jwt_secret_key

  # Define the routes where JWT tokens should be dispatched (created)
  config.dispatch_requests = [
    ['POST', %r{^/login$}],
    ['POST', %r{^/graphql$}]
  ]

  # Define the routes where JWT tokens should be revoked (invalidated)
  config.revocation_requests = [
    ['DELETE', %r{^/logout$}]
  ]

  # Set the expiration time for the JWT tokens (e.g., 1 day)
  config.expiration_time = 1.day.to_i
end

# Questions:
# 1. What role does Warden play in this setup?
# 2. How does the JWT middleware interact with Warden?
# 3. Why is it necessary to define the expiration time for JWT tokens?

## Step 6: Update User Model
# Add JWT revocation strategies to the User model
# app/models/user.rb
class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable,
         :jwt_authenticatable, jwt_revocation_strategy: JwtDenylist
end

# Questions:
# 1. What does the `jwt_revocation_strategy` option do?
# 2. How does Devise's `jwt_authenticatable` module work?
# 3. Why is it important to include `:validatable` in the Devise modules?

## Step 7: Update JwtDenylist Model
# app/models/jwt_denylist.rb
class JwtDenylist < ApplicationRecord
  include Devise::JWT::RevocationStrategies::Denylist

  self.table_name = 'jwt_denylists'
end

# Run the migration
rails db:migrate

# Questions:
# 1. What is the purpose of including `Devise::JWT::RevocationStrategies::Denylist` in the model?
# 2. Why do we explicitly set `self.table_name`?
# 3. How does the `JwtDenylist` model interact with the User model?

## Step 8: Create Sessions Controller for JWT
# create sessions_controller.rb
touch app/controllers/sessions_controller.rb

# app/controllers/sessions_controller.rb
# app/controllers/sessions_controller.rb
class SessionsController < Devise::SessionsController
  respond_to :json

  private

  def respond_with(resource, _opts = {})
    token = extract_token_from_header
    if token.nil?
      render json: { message: 'No token found, please provide a token', user: nil }, status: :unauthorized
    elsif current_user
      render json: { message: 'Logged in successfully', user: current_user }, status: :ok
    else
      render json: { message: 'Please login', user: nil }, status: :unauthorized
    end
  end

  def respond_to_on_destroy
    token = extract_token_from_header
    if token
      handle_token_revocation(token)
    else
      render json: { message: 'No token provided' }, status: :bad_request
    end
  end

  def extract_token_from_header
    request.headers['Authorization']&.split(' ')&.last
  end

  def handle_token_revocation(token)
    begin
      jwt_payload = JWT.decode(token, Rails.application.credentials.jwt_secret_key).first
      user = User.find(jwt_payload['sub'])
      if user
        Warden::JWTAuth::RevocationStrategies::Denylist.revoke(jwt_payload, user)
        render json: { message: 'Logged out successfully' }, status: :ok
      else
        render json: { message: 'Invalid token or user not found' }, status: :unauthorized
      end
    rescue JWT::DecodeError => e
      render json: { message: "Invalid token: #{e.message}" }, status: :unauthorized
    end
  end
end


# Questions:
# 1. What is the purpose of overriding `respond_with` and `respond_to_on_destroy`?
# 2. How does the `SessionsController` handle JWT token decoding?
# 3. Why is it important to respond with JSON in an API?

## Step 9: Update Routes
# config/routes.rb
Rails.application.routes.draw do
  devise_for :users, controllers: { sessions: 'sessions' }
  if Rails.env.development?
    mount GraphiQL::Rails::Engine, at: "/graphiql", graphql_path: "/graphql"
  end
  post "/graphql", to: "graphql#execute"
end

# Questions:
# 1. How does the `devise_for` method configure routes for Devise?
# 2. Why do we mount the GraphiQL engine in development?
# 3. What is the purpose of the `/graphql` route?

## Step 10: Update GraphQL User Type and Mutations

# app/graphql/types/user_type.rb
module Types
  class UserType < Types::BaseObject
    field :id, ID, null: false
    field :name, String, null: false
    field :email, String, null: false
    field :token, String, null: true

    def token
      object.token
    end
  end
end

# Questions:
# 1. What is the significance of adding a `token` field to the `UserType`?
# 2. How does the `token` method work in this context?
# 3. Why is it important to define types in GraphQL?

# Replace SignIn and add Logout mutation class

# app/graphql/mutations/user_mutations.rb
module Mutations
  module UserMutations
    class SignIn < Mutations::BaseMutation
      argument :email, String, required: true
      argument :password, String, required: true
  
      field :message, String, null: false
      field :user, Types::UserType, null: true
      field :token, String, null: true
  
      def resolve(email:, password:)
        user = User.find_for_authentication(email: email)
        unless user&.valid_password?(password)
          return {
            message: "Invalid email or password",
            user: nil,
            token: nil
          }
        end
  
        token = Warden::JWTAuth::UserEncoder.new.call(user, :user, nil).first
        {
          message: "Signed in successfully",
          user: user,
          token: token
        }
      end
    end

    class Logout < Mutations::BaseMutation
      field :message, String, null: false

      def resolve
        token = context[:jwt_token]
        if token.nil?
          Rails.logger.warn "Logout attempt with no token provided"
          return { message: "No token provided" }
        end

        begin
          jwt_payload = JWT.decode(token, Rails.application.credentials.jwt_secret_key).first
          user = User.find(jwt_payload['sub'])

          if user
            JwtDenylist.create!(jti: jwt_payload['jti'], exp: Time.at(jwt_payload['exp']))
            Rails.logger.info "Token revoked for user: #{user.id}"
            { message: "Logged out successfully" }
          else
            Rails.logger.error "Invalid token or user not found for token: #{token}"
            { message: "Invalid token or user not found" }
          end
        rescue JWT::DecodeError => e
          Rails.logger.error "Invalid token: #{e.message}"
          raise GraphQL::ExecutionError, "Invalid token: #{e.message}"
        end
      end
    end

  end
end

# Questions:
# 1. How does the SignIn mutation generate a JWT token?
# 2. What is the purpose of `GraphQL::ExecutionError` in this context?
# 3. Why is `define_singleton_method` used to add the token method to the user instance?
# 4. How is JwtDenylist and how does it work?

## Step 11: Update GraphQL Controller

# Replace graphql_controller.rb
# Change `RailsApiSchema` below to match what you
# have in app/graphql/xx_schema.rb file, the class definition

# app/controllers/graphql_controller.rb

# frozen_string_literal: true

# frozen_string_literal: true

class GraphqlController < ApplicationController
  before_action :authenticate_user!, unless: -> { graphql_authentication_action? }

  def execute
    variables = prepare_variables(params[:variables])
    query = params[:query]
    operation_name = params[:operationName]
    context = {
      current_user: set_current_user,
      jwt_token: request.headers['Authorization']&.split(' ')&.last
    }

    result = RailsApiSchema.execute(query, variables: variables, context: context, operation_name: operation_name)
    render json: result
  rescue StandardError => e
    raise e unless Rails.env.development?
    handle_error_in_development(e)
  end

  private

  def authenticate_user!
    token = request.headers['Authorization']&.split(' ')&.last
    unless token
      render json: { error: 'Unauthorized' }, status: :unauthorized
      return
    end

    begin
      jwt_payload = JWT.decode(token, Rails.application.credentials.jwt_secret_key).first
      @current_user = User.find(jwt_payload['sub'])
    rescue JWT::DecodeError
      render json: { error: 'Invalid token' }, status: :unauthorized
    rescue ActiveRecord::RecordNotFound
      render json: { error: 'User not found' }, status: :unauthorized
    end
  end

  def set_current_user
    token = request.headers['Authorization']&.split(' ')&.last
    return nil unless token

    begin
      jwt_payload = JWT.decode(token, Rails.application.credentials.jwt_secret_key).first
      User.find(jwt_payload['sub'])
    rescue JWT::DecodeError, ActiveRecord::RecordNotFound
      nil
    end
  end

  def graphql_authentication_action?
    sign_up_query? || sign_in_query?
  end

  def sign_up_query?
    params[:query]&.include?('signUp')
  end

  def sign_in_query?
    params[:query]&.include?('signIn')
  end

  def prepare_variables(variables_param)
    case variables_param
    when String
      if variables_param.present?
        JSON.parse(variables_param) || {}
      else
        {}
      end
    when Hash
      variables_param
    when ActionController::Parameters
      variables_param.to_unsafe_hash
    when nil
      {}
    else
      raise ArgumentError, "Unexpected parameter: #{variables_param}"
    end
  end

  def handle_error_in_development(e)
    logger.error e.message
    logger.error e.backtrace.join("\n")
    render json: { error: { message: e.message, backtrace: e.backtrace }, data: {} }, status: 500
  end
end



# Questions:
# 1. How does the GraphqlController authenticate users?
# 2. What is the purpose of the `context` hash in the `execute` method?
# 3. Why do we need to handle errors differently in development?
# 4. What does the `authenticate_user` definition achieve

## Step 12: Test Your Setup
# Start the server
rails server

# Navigate to http://localhost:3000/graphiql in your browser to access the GraphiQL interface.

# Test the sign-up mutation
mutation {
  signUp(input: {
    name: "Dave Doe",
    email: "john@example.com",
    password: "password123"
  }) {
    id
    name
    email
  }
}

# Test the sign-in mutation
# make of note of the token it will be used for any other requests
mutation {
  signIn(input: {
    email: "john@example.com",
    password: "password123"
  }) {
    id
    name
    email
    token
  }
}

# For any other requests, bottom of graphiql, click on Headers
# and enter below, it is also possible to persist the token by
# clicking on the settings cog buttom left of graphiql browser

{
  "Authorization": "Bearer your-token-goes-here"
}


# Fetch a single user by ID
query {
  user(id: 1) {
    id
    name
    email
  }
}

# Fetch all users
query {
  users {
    id
    name
    email
  }
}

# Test the update user mutation
mutation {
  updateUser(input: {
    id: 1,
    name: "John Smith",
    email: "john.smith@example.com",
    password: "newpassword123"
  }) {
    id
    name
    email
  }
}

# Test the delete user mutation
mutation {
  deleteUser(input: {id: 1}) {
    id
    name
    email
  }
}

# Test the reset password mutation
mutation {
  resetPassword(input: {
    email: "john@example.com"
  }) {
    id
    name
    email
  }
}

# Logout mutation

# This will set the token as denylist forcing a new generation for next time.
mutation {
  logout(input: {}) {
    message
  }
}
# Questions:
# 1. How do you test GraphQL mutations using GraphiQL?
# 2. What would you do if a mutation fails?
# 3. Why is it important to test each mutation thoroughly?

```
