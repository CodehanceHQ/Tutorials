# JWT Install & Setup

### Understanding JWT

```
# Understanding JWT

# Questions:
# 1. What is JWT (JSON Web Token) and how does it work in the context of web authentication?
# 2. How is the payload of a JWT structured and what information does it typically contain?
# 3. Explain the process of encoding and decoding a JWT. What role do secret keys play in this process?
# 4. What are some common use cases for JWT in a Rails application and how do you ensure its security?
```

### Step 1: Add Devise JWT Gem

```
# Open your Gemfile and add the following gems:
gem 'devise-jwt', '~> 0.11.0'

# Install the gems
bundle install
```

### Step 2: Create JwtDenylist Model

```
# Generate a model for JWT denylist
rails generate model JwtDenylist jti:string:index exp:datetime

# Questions:
# 1. What is the purpose of the `JwtDenylist` model?
# 2. What does the `jti` attribute represent in the context of JWTs?
# 3. Why is it useful to index the `jti` attribute?
# 4. How does the `exp` attribute help in managing JWTs?
```

### Step 3: Add JWT Secret to Credentials

```
# Run the following command to add JWT secret to your credentials

# Run this in terminal and copy the output
rails secret

# Run this in first command tab
# This will launch into VSCode waiting for update
# IMPORTANT: close file after changes for --wait to be terminated
EDITOR="code --wait" bin/rails credentials:edit

# In VSCode, update to match below
secret_key_base: leave_this_as_was_generated
jwt_secret_key: enter_your_secret_key_copied_from_cmd

# Note: after saving the file will only contain encrypted one liner, when
# decrypted then it shows more info.

# Questions:
# 1. Why is it important to keep the JWT secret key secure?
# 2. What is the purpose of the `rails secret` command?
# 3. How do you edit Rails credentials?
# 4. What is master.key, where is it, and when is it generated?
# 5. Why is it important for master.key to be in .gitignore?
# 6. How would fellow collaborative coders get master.key?
# 7. Is it safe to commit credentials.yml.enc and why?
```

### Step 4: Configure Devise for JWT

```
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
# 4. How does setting an expiration time help in securing JWTs?
```

### Step 5: Include JWT Middleware

```
# Add JWT middleware to the Warden configuration
# config/initializers/warden.rb

# Generate config/initializers/warden.rb
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
# 4. How do `dispatch_requests` and `revocation_requests` work in this configuration?
```

### Step 6: Update User Model

```
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
# 4. How does the User model interact with the JwtDenylist model?
```

### Step 7: Update JwtDenylist Model

```
# app/models/jwt_denylist.rb
class JwtDenylist < ApplicationRecord
  include Devise::JWT::RevocationStrategies::Denylist

  self.table_name = 'jwt_denylists'
end

# Run the migration
rails db:migrate

# Questions:
# 1. What does jwt_denylist do? what is its purpose?
# 2. What is the purpose of including `Devise::JWT::RevocationStrategies::Denylist` in the model?
# 3. Why do we explicitly set `self.table_name`?
# 4. How does the `JwtDenylist` model interact with the User model?
# 5. What is the impact of running `rails db:migrate` in this setup?
```

### Step 8: Create Sessions Controller for JWT

```
# create sessions_controller.rb
touch app/controllers/sessions_controller.rb

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
```

### Step 9: Update Routes
```
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
```

### Step 10: Start server
```
# Check everything is working ok with no errors

rails server

# visit url
http://127.0.0.1:3000/graphiql
```

# Conclusion

```
In this guide, we have installed and configured JWT for a Rails application using Devise. We began by adding the necessary gems and creating a model for the JWT denylist.

We then securely added the JWT secret key to our Rails credentials and configured Devise to use JWT for authentication. We also included JWT middleware in the Warden configuration and updated the User model to handle JWT revocation strategies.

Finally, we set up the JwtDenylist model and ran the migrations to update the database schema. By following these steps, you have successfully integrated JWT into your Rails application, providing a secure and efficient method for handling authentication.
```
