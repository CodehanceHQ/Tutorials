## GraphQL User Sign In Mutation

### Step 1: Send SignIn Mutation
```
# Visit your graphQL:
http://127.0.0.1:3000/graphiql

# Paste below and submit
mutation {
  signIn(input: {
    email: "john.doe@example.com",
    password: "Password@123"
  }) {
    data {
      id
      fullName
      email
      telephone
    }
    errors,
    message,
    httpStatus
  }
}

# Expect to see error
"message": "Field 'signIn' doesn't exist on type 'Mutation'"

# Questions:
# 1. What are the input arguments in the `signIn` mutation and what do they represent?
# 2. What return fields are expected from the `signIn` mutation?
# 3. Why did we receive the error "Field 'signIn' doesn't exist on type 'Mutation'"?
# 4. How would you describe the purpose of the `signIn` mutation?
# 5. Why is this a mutation and not a query?
```

### Step 2: Create Schema type
```
# app/graphql/types/mutation_type.rb

module Types
  class MutationType < Types::BaseObject
    # add below existing fields
    field  :signIn, mutation: Mutations::UserMutations::SignIn
  end
end

# Questions:
# 1. What does adding the `signIn` field to the MutationType class accomplish?
# 2. What error do we get when we re-run the signIn mutation?
# 3. What does `"message": "uninitialized constant Mutations::UserMutations::SignIn"` mean?
# 4. How does adding a mutation field help route the mutation?
```

### Step 3: Create SignIn mutation
```
# path: app/graphql/mutations/user_mutations.rb

module Mutations
  module UserMutations
    # ... existing type class
    class SignIn < Mutations::BaseMutation
      # Define the input arguments for the mutation
      argument :email, String, required: true
      argument :password, String, required: true

      # Define the return types for the mutation
      field :data, Types::UserType, null: true
      field :errors, [String], null: true
      field :message, String, null: false
      field :http_status, Integer, null: false

      def resolve(email:, password:)
        user = User.find_by(email: email)

        if user&.valid_password?(password)
          {
            data: user,
            errors: [],
            message: "User signed in successfully",
            http_status: 200 # OK
          }
        else
          {
            data: nil,
            errors: ["Invalid email or password"],
            message: "User sign in failed",
            http_status: 401 # Unauthorized
          }
        end
      end
    end
  end
end

# Questions:
# 1. What is the purpose of defining arguments in a GraphQL mutation?
# 2. What are fields in a GraphQL mutation?
# 3. What does `&` mean here `if user&.valid_password?(password)`
# 4. What is the http_status code for Unauthorized?
# 5. What response do you get when you run this mutation?
```

### Step 4: Ensure UserType is Defined
```
# Ensure the UserType is already defined as it is shared between SignUp and SignIn mutations

touch app/graphql/types/user_type.rb

module Types
  class UserType < Types::BaseObject
    field :id, ID, null: false
    field :full_name, String, null: false
    field :email, String, null: false
    field :telephone, String, null: false
  end
end

```

### Step 5: Send Mutation again
```
# http://127.0.0.1:3000/graphiql

re-submit signIn and this time it should authenticate the user and return the data along with a token.

### Step 6: Validation Handling
# Attempt to post this mutation with invalid data

mutation {
  signIn(input: {
    email: "invalid email",
    password: "wrongpassword"
  }) {
    data {
      id
      fullName
      email
      telephone
    }
    token,
    message,
    errors,
    httpStatus
  }
}

# Question 1: What validation errors did you encounter?
# Question 2: How does the mutation handle invalid credentials?
# Question 3: What HTTP status code is returned for invalid credentials?
# Question 4: Why is it important to provide clear error messages for authentication failures?
```

### Step 6: Run Spec & Coverage
```
# Run spec to generate updated coverage
bundle exec rspec

# Run coverage
open coverage/index.html

# Questions:
# 1. Why did we have to run `bundle exec rspec` before the coverage?
# 2. What is the result of coverage for `user_mutations.rb`?
# 3. What lines in that file are missing test coverage?
# 4. What steps do we need to cover those with tests?
```

### Conclusion
```
# This guide walks you through setting up a GraphQL controller, understanding the request lifecycle, creating schema types, and implementing the SignIn mutation with appropriate validations.

# It also includes testing your mutation with sample queries and ensuring it authenticates the user correctly. 

By following these steps, you can effectively handle GraphQL requests and responses in a Rails application, ensuring secure and validated user sign-ins.
```