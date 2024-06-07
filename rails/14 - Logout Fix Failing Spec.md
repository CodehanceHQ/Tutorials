## GraphQL User SignOut Mutation

### Step 1: Send SignOut Mutation
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

# Expect to see error
{
  "error": "Unauthorized"
}

# Update header info and send again
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
# app/graphql/mutations/user_mutations.rb

# Update `user_mutations.rb` to include:

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

# Questions:
# 1: What role does JwtDenylist play in logging out?
# 2: What is a `context` in rails controller?
# 3: How did we define the contexts in graphql controller for user & jwt_payload
```

### Step 4: Run spec file
```
# specific
rspec spec/requests/auth/logouts_spec.rb

# all
rspec
```

### Run mutation Query
```
# visit:
http://127.0.0.1:3000/graphiql

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

# provide authorization header
{
  "Authorization": "Bearer your token here"
}

# Questions
# 1: Are you able to logout?
# 2: Try logging out again using same details, what error did you receive?
```