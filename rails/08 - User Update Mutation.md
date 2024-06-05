## GraphQL User Update Mutation

### Step 1: Send UpdateUser Mutation
```
# Visit your graphQL:
http://127.0.0.1:3000/graphiql

# Paste below and submit
mutation {
  updateUser(input: {
    id: 1,
    fullName: "John Updated",
    email: "john.updated@example.com",
    telephone: "07545676788"
  }) {
    data {
      id
      fullName
      email
      telephone
    }
    message,
    errors,
    httpStatus
  }
}

# Expect to see error
"message": "Field 'updateUser' doesn't exist on type 'Mutation'"

# Questions:
# 1. What are the input arguments in the `updateUser` mutation and what do they represent?
# 2. What return fields are expected from the `updateUser` mutation?
# 3. Why did we receive the error "Field 'updateUser' doesn't exist on type 'Mutation'"?
# 4. What is the next step in fixing the error to ensure the `updateUser` mutation works correctly?
# 5. Why is this a mutation and not a query?
```

### Step 2: Create Schema type
```
# app/graphql/types/mutation_type.rb

module Types
  class MutationType < Types::BaseObject
    # add below existing fields
    field  :updateUser, mutation: Mutations::UserMutations::UpdateUser
  end
end

# Questions:
# 1. What does the field type here accomplish?
# 2. What error do we get when we re-run the updateUser mutation?
# 3. What does `"message": "uninitialized constant Mutations::UserMutations::UpdateUser"` mean?
# 4. What next step do we need to take?
```

### Step 3: Create UpdateUser mutation
```
# path: app/graphql/mutations/user_mutations.rb

module Mutations
  module UserMutations
    # ... existing type class
    class UpdateUser < Mutations::BaseMutation
      # Define the input arguments for the mutation
      argument :id, ID, required: true
      argument :full_name, String, required: true
      argument :email, String, required: true
      argument :telephone, String, required: true

      # Define the return types for the mutation
      field :data, Types::UserType, null: true
      field :errors, [String], null: true
      field :message, String, null: false
      field :http_status, Integer, null: false

      def resolve(id:, full_name:, email:, telephone:)
        user = User.find(id)

        if user.update(
          full_name: full_name,
          email: email,
          telephone: telephone
        )
          {
            data: user,
            errors: [],
            message: "User updated successfully",
            http_status: 200 # OK
          }
        else
          {
            data: nil,
            errors: user.errors.full_messages,
            message: "User update failed",
            http_status: 422 # Unprocessable Entity
          }
        end
      end
    end
  end
end

# Question 1: What is the purpose of defining arguments in a GraphQL mutation?
# Question 2: What are fields in a GraphQL mutation?
# Question 3: Explain the role of HTTP status codes in the UpdateUser mutation? 
# Question 4: What error message do we get when we run this mutation?
# Question 5: Why is it necessary to create a `Types::UserType`?
# Question 6: How did we call rails update method?
```

### Step 4: Ensure UserType is Defined
```
# Ensure the UserType is already defined as it is shared between SignUp and UpdateUser mutations

touch app/graphql/types/user_type.rb

module Types
  class UserType < Types::BaseObject
    field :id, ID, null: false
    field :full_name, String, null: false
    field :email, String, null: false
    field :telephone, String, null: false
  end
end

# Question 1: What is the purpose of defining fields in the `UserType` class?
# Question 2: Why do we set `null: false` for some fields in the `UserType`?
# Question 3: What role does the `Types::UserType` play in the GraphQL schema?
# Question 4: Explain the significance of the `field` keyword in the `UserType` class.
```

### Step 5: Send Mutation again
```
# http://127.0.0.1:3000/graphiql

re-submit updateUser and this time it updates the data in the database.

### Step 6: Validation Handling
# Attempt to post this mutation with invalid data

mutation {
  updateUser(input: {
    id: 1,
    fullName: "",
    email: "invalid email",
    telephone: "invalid telephone"
  }) {
    data {
      id
      fullName
      email
      telephone
    }
    message,
    errors,
    httpStatus
  }
}

# Question 1: What validation errors did you encounter?
# Question 2: Explain how our validation is being picked up
# Question 3: Explain how validation is returned back as response
```

### Step 6: Run Spec & Coverage
```
# Run spec to generate updated coverage
bundle exec rspec

# Run coverage
open coverage/index.html

# Question 1: Why did we have to run `bundle exec rspec` before the coverage?
# Question 2: What is the result of coverage for `user_mutations.rb`?
# Question 3: What lines in that file are missing test coverage?
# Question 4: What steps do we need to cover those with tests?
```

### Conclusion
```
# This guide walks you through setting up a GraphQL controller, understanding the request lifecycle, creating schema types, and implementing the UpdateUser mutation with appropriate validations. 

It also includes testing your mutation with sample queries and ensuring it updates data correctly in the database. By following these steps, you can effectively handle GraphQL requests and responses in a Rails application, ensuring secure and validated user updates.
```