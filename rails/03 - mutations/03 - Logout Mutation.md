## GraphQL User SignOut Mutation

### Step 1: Login to receive token
```
# Visit your graphQL:
http://127.0.0.1:3000/graphiql

mutation {
  signIn(input: {
    email: "woric79290@kernuo.com",
    password: "PAssword@123"
  }) {
    data {
      id
      fullName
      email
      telephone
    }
    errors,
    message,
    httpStatus,
    token
  }
}
```

### Step 2: Send SignOut Mutation
```
# Visit your graphQL:
http://127.0.0.1:3000/graphiql

# Paste below and submit
mutation {
  logout(input: {}) {
    data {
      id,
      fullName,
      email,
      telephone
    }
    message,
    errors,
    httpStatus
  }
}

# Update header info
{
  "Authorization": "Bearer your token here"
}

# Then you will see:
"message": "Field 'logout' doesn't exist on type 'Mutation'"
```

### Step 2: Update mutation type
```
# app/graphql/types/mutation_type.rb

module Types
  class MutationType < Types::BaseObject
    # leave previous fields
    field  :logout, mutation: Mutations::UserMutations::Logout
  end
end

# Send again to see this error:
"message": "uninitialized constant Mutations::UserMutations::Logout"
```

### Step 3: Create Logout Mutation
```
touch app/graphql/mutations/user_mutations/logout.rb

# path: app/graphql/mutations/user_mutations/logout.rb

module Mutations
  module UserMutations
    class Logout < Mutations::BaseMutation
      # Define the return types for the mutation
      field :data, Types::UserType, null: true
      field :errors, [String], null: true
      field :message, String, null: false
      field :http_status, Integer, null: false

      def resolve
        # Get the current user from the context defined in graphql controller
        user = context[:current_user]
        jwt_payload = context[:jwt_payload]

        # Check if user already added to denylist
        if user
          # Check if user token is already in the denylist
          denied = JwtDenylist.find_by(jti: jwt_payload['jti'])
          if denied 
            {
              data: nil,
              errors: ["You are already logged out"],
              message: "Already logged out",
              http_status: 401 # Unauthorized
            }
          else
            # Add user token to denylist (Logout user)
            JwtDenylist.create(jti: jwt_payload['jti'])
            {
              data: user,
              errors: [],
              message: "Successfully logged out",
              http_status: 200 # OK
            }
          end
        end
      end
    end
  end
end


# Questions:
# 1: What role does JwtDenylist play in logging out?
# 2: What is a `context` in rails controller?
# 3: How did we define the contexts in graphql controller for user & jwt_payload
```