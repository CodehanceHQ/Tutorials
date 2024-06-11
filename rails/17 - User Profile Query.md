# User Profile Query

### Step 1: Send User Profile Query
```
# Visit your graphQL:
http://127.0.0.1:3000/graphiql

# Paste below and submit
query {
  userProfile {
    data {
      id
      fullName
      email
      telephone
    }
    errors
    message
    httpStatus
  }
}

{
  "Authorization": "Bearer token-here"
}

# Questions:
# 1: Why have we used query instead of mutation?
# 2: Why is input empty?
# 3: How do we know which user is making the request?
# 4: What error do we get with this request?
```

### Step 2: Add query type
```
field :user_profile, resolver: Resolvers::UserResolver

# Questions:
# 1: What do you understand from this change?
# 2: What will be contained in `UserResolver`
```

### Step 3: Define Profile type
```
touch app/graphql/types/user_profile_type.rb

module Types
  class UserProfileType < Types::BaseObject
    field :data, Types::UserType, null: true
    field :errors, [String], null: true
    field :message, String, null: false
    field :http_status, Integer, null: false
  end
end

# Questions:
# 1: What is meant by `[String]` above?
# 2: What will be the structure of data field above?
```

### Step 4: User Profile Resolver
```
# create a user resolver
touch app/graphql/resolvers/user_resolver.rb

module Resolvers
  class UserResolver < Resolvers::BaseResolver
    # Define the return types for the query
    type Types::UserProfileType, null: false

    def resolve()
      user = context[:current_user]

      if user
        {
          data: user,
          errors: [],
          message: "Profile fetched successfully",
          http_status: 200 # OK
        }
      else
        {
          data: nil,
          errors: ["User not found"],
          message: "User not found",
          http_status: 404 # Not Found
        }
      end
    end
  end
end

# Questions:
# 1: Where do we get the return type from?
# 2: Where do we get the current_user from? where is it defined?
```