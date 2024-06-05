# Install & Setup Devise

```
# This guide will walk you through performing CRUD (Create, Read, Update, Delete) operations for Devise User.

# Step-by-Step Guide

# Step 1: Install Devise
# Add Devise gem to the Gemfile
gem 'devise', '~> 4.8'

# Install the gems
bundle install

# Run the Devise installation generator
rails generate devise:install

# Generate a User model with Devise
rails generate devise User

# Add the full name attribute to the User model
rails generate migration AddFullNameToUsers name:string

# Run the migrations
rails db:migrate

# Question: What command do you use to install Devise in your Rails application?
# Question: What command do you use to generate a User model with Devise?
# Question: Why do we need to run the command `rails generate migration AddFullNameToUsers name:string`?
# Question: What command do you use to run the migrations?
# Question: What happens when the command `rails db:migrate` runs?
# Question: What files are created by `rails generate devise:install`?

# Step 2: Define GraphQL Types, Mutations, and Queries

# app/graphql/types/user_type.rb
module Types
  class UserType < Types::BaseObject
    field :id, ID, null: false
    field :name, String, null: false
    field :email, String, null: false
  end
end

# Question: Explain the significance of the null: false option in the field definitions?

# Step 3: Define user queries

mkdir -p app/graphql/queries &&
touch app/graphql/queries/user_queries.rb

# app/graphql/queries/user_queries.rb
module Queries
  module UserQueries
    extend ActiveSupport::Concern

    included do
      # Fetch a single user by ID
      field :user, Types::UserType, null: false do
        argument :id, GraphQL::Types::ID, required: true
      end

      def user(id:)
        User.find(id)
      end

      # Fetch all users
      field :users, [Types::UserType], null: false

      def users
        User.all
      end
    end
  end
end

# Question: explain why we used extend ActiveSupport::Concern?
# Question: explain what `included do` will do?
# Question: How do you define a query to fetch a single user by ID?
# Question: What methods are used to fetch a single user and all users?

# Step 3: Create query_types to use include Queries::UserQueries

# app/graphql/types/query_type.rb
# Path: app/graphql/types/query_type.rb
module Types
  class QueryType < Types::BaseObject

    # Dynamically require all query files in the queries directory
    Dir[Rails.root.join('app/graphql/queries/**/*.rb')].each { |file| require file }

    # Dynamically include all query modules
    Queries.constants.each do |query|
      include Queries.const_get(query)
    end
  end
end

# Question: Explain what this file does?

# create user_mutations.rb

touch app/graphql/mutations/user_mutations.rb

# app/graphql/mutations/user_mutations.rb
module Mutations
  module UserMutations
    class SignUp < Mutations::BaseMutation
      argument :name, String, required: true
      argument :email, String, required: true
      argument :password, String, required: true

      type Types::UserType

      def resolve(name:, email:, password:)
        User.create!(name: name, email: email, password: password)
      end
    end

    class SignIn < Mutations::BaseMutation
      argument :email, String, required: true
      argument :password, String, required: true

      type Types::UserType

      def resolve(email:, password:)
        user = User.find_for_authentication(email: email)
        return unless user&.valid_password?(password)

        user
      end
    end

    class UpdateUser < Mutations::BaseMutation
      argument :id, ID, required: true
      argument :name, String, required: false
      argument :email, String, required: false
      argument :password, String, required: false

      field :message, String, null: false
      field :user, Types::UserType, null: false

      def resolve(id:, name: nil, email: nil)
        user = User.find(id)
        if user.update(name: name, email: email)
          {
            message: "User updated successfully",
            user: user
          }
        else
          raise GraphQL::ExecutionError.new("Error updating user", extensions: user.errors.to_hash)
        end
      end
    end

    class DeleteUser < Mutations::BaseMutation
      argument :id, ID, required: true

      field :message, String, null: false
      field :user, Types::UserType, null: true

      def resolve(id:)
        user = User.find_by(id: id)
        if user
          user.destroy
          {
            message: "User deleted successfully",
            user: user
          }
        else
          {
            message: "User not found",
            user: nil
          }
        end
      end
    end

    class ResetPassword < Mutations::BaseMutation
      argument :email, String, required: true

      field :message, String, null: false
      field :user, Types::UserType, null: true

      def resolve(email:)
        user = User.find_by(email: email)
        if user
          user.send_reset_password_instructions
          {
            message: "Reset password instructions have been sent to #{email}",
            user: user
          }
        else
          {
            message: "User not found",
            user: nil
          }
        end
      end
    end
  end
end

# Question: How do you define a signUp mutation and what arguments does it require?
# Question: How do you define a signIn mutation and what arguments does it require?
# Question: How do you define an update user mutation and what arguments does it require?
# Question: How do you define a delete user mutation and what arguments does it require?
# Question: How do you define a reset password mutation and what arguments does it require?

# Step 3: Mutation types

# app/graphql/types/mutation_type.rb

module Types
  class MutationType < Types::BaseObject
    # Require all mutation files in the mutations directory
    Dir[Rails.root.join('app/graphql/mutations/**/*.rb')].each { |file| require file }

    # Iterate through the Mutations module and dynamically define fields
    Mutations.constants.each do |namespace|
      namespace_module = Mutations.const_get(namespace)
      next unless namespace_module.is_a?(Module)

      namespace_module.constants.each do |mutation|
        mutation_class = namespace_module.const_get(mutation)
        if mutation_class.is_a?(Class) && mutation_class < Mutations::BaseMutation
          # e.g: field :sign_up, mutation: Mutations::UserMutations::SignUp
          field mutation.to_s.underscore.to_sym, mutation: mutation_class
        end
      end
    end
  end
end

# Question: What is a mutation_type file?
# Question: Explain what the code above does?

# Step 4: Start the Server and Test Your Setup

# Start the server
rails server

# Navigate to http://localhost:3000/graphiql in your browser to access the GraphiQL interface.

# Question: What command do you use to start the Rails server?

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

# Test the sign-in mutation
mutation {
  signIn(input: {
    email: "john@example.com",
    password: "password123"
  }) {
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

# Question: How do you test the sign-up mutation in GraphiQL?
# Question: How do you test the query to fetch a single user by ID in GraphiQL?
# Question: How do you test the query to fetch all users in GraphiQL?
# Question: How do you test the sign-in mutation in GraphiQL?
# Question: How do you test the update user mutation in GraphiQL?
# Question: How do you test the delete user mutation in GraphiQL?
# Question: How do you test the reset password mutation in GraphiQL?

```
