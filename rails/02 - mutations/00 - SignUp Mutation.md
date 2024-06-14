## GraphQL User Mutation

### Understanding GraphQL Request Life Cycle
```
# Questions:
# 1. What is the role of the GraphQL controller in handling GraphQL requests?
# 2. What is a GraphQL query? find an example
# 3. What is a GraphQL mutation? find an example
# 2. How does a GraphQL query or mutation get routed from the controller to the schema?
# 3. What is the purpose of the query type and mutation type in a GraphQL schema?
```


### Step 1: Send Signup Mutation
```
# Visit your graphQL:
http://127.0.0.1:3000/graphiql

# Paste below and submit
mutation {
  signUp(input: {
    fullName: "Jonn Doe",
    email: "john.doe@example.com",
    password: "Password123",
    confirmPassword: "Password123",
    telephone: "07545676777"
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
"message": "Field 'signUp' doesn't exist on type 'Mutation'"

# Questions:
# 1. What are the input arguments in the `signUp` mutation and what do they represent?
# 2. What return fields are expected from the `signUp` mutation?
# 3. Why did we receive the error "Field 'signUp' doesn't exist on type 'Mutation'"?
# 4. What is the next step in fixing the error to ensure the `signUp` mutation works correctly?
# 5. Why is this a mutation and not a query?
```

### Step 2: Create Schema type
```
# app/graphql/types/mutation_type.rb

module Types
  class MutationType < Types::BaseObject
    field  :signUp, mutation: Mutations::UserMutations::SignUp
  end
end

# Questions:
# 1. What does the field type here accomplish?
# 2. What error do we get when we re-run the signUp mutation?
# 3. What does `"message": "uninitialized constant Mutations::UserMutations"` mean?
# 4. What next step do we need to take?
```

### Step 3: Create SignUp mutation
```
mkdir app/graphql/mutations/user_mutations
touch app/graphql/mutations/user_mutations/sign_up.rb

# path: app/graphql/mutations/user_mutations/sign_up.rb

module Mutations
  module UserMutations
    class SignUp < Mutations::BaseMutation
      # Define the input arguments for the mutation
      argument :full_name, String, required: true
      argument :email, String, required: true
      argument :password, String, required: true
      argument :confirm_password, String, required: true
      argument :telephone, String, required: true

      # Define the return types for the mutation
      field :data, Types::UserType, null: true
      field :errors, [String], null: true
      field :message, String, null: false
      field :http_status, Integer, null: false

      def resolve(full_name:, email:, password:, confirm_password:, telephone:)
        user = User.new(
          full_name: full_name,
          email: email,
          password: password,
          password_confirmation: confirm_password,
          telephone: telephone
        )

        if user.save
          {
            data: user,
            errors: [],
            message: "User created successfully",
            http_status: 200 # OK
          }
        else
          {
            data: nil,
            errors: user.errors.full_messages,
            message: "User creation failed",
            http_status: 422 # Unprocessable Entity
          }
        end
      end
    end
  end
end

# Question 1: What is the purpose of defining arguments in a GraphQL mutation?
# Question 2: What are fields in a GraphQL mutation?
# Question 3: Explain the role of HTTP status codes in the SignUp mutation? 
# Question 4: What error message do we get when we run this mutation?
# Question 5: Why is it necessary to create a `Types::UserType`?
```

### Step 4: Create UserType
```
touch app/graphql/types/user_type.rb

module Types
  class UserType < Types::BaseObject
    field :id, ID, null: false
    field :full_name, String, null: false
    field :email, String, null: false
    field :password, String, null: false
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

re-submit signUp and this time it persists into database.
```

### Step 6: Devise inherited validation
```
# Attempt to post this empty data mutation

mutation {
  signUp(input: {
    fullName: "",
    email: "",
    password: "",
    confirmPassword: "",
    telephone: ""
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
# Question 2: What happens when you enter valid email but non matching passwords?
# Question 3: Since we have not written these validations where are they coming from?
# Question 4: In rails app where are validations created?
```

### Conclusion
```
This guide walks you through setting up a GraphQL controller, understanding the request lifecycle, replacing controller content, creating schema types, and implementing the SignUp mutation with appropriate validations. It also includes testing your mutation with sample queries and ensuring it persists data correctly into the database. By following these steps, you can effectively handle GraphQL requests and responses in a Rails application, ensuring secure and validated user sign-ups.
```