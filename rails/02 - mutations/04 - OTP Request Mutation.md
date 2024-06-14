## GraphQL OTP request Mutation

### Step 1: Send OTP request Mutation
```
# Visit your graphQL:
http://127.0.0.1:3000/graphiql

mutation {
  otpRequest(input: {
    email: "john16@example.com"
  }) {
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

# Questions:
# 1: Why dont we send headers for OTP / password reset requests?
# 2: Why do we need users email address?
# 3: Where can we store the OTP?
```

### Step 2: Create mutatble type
```
touch app/graphql/mutations/user_mutations/logout.rb

# path: app/graphql/mutations/user_mutations/logout.rb

module Types
  class MutationType < Types::BaseObject
    # leave the rest
    field  :otpRequest, mutation: Mutations::UserMutations::OtpRequest 
  end
end
```

### Step 3: Create OTP table and fields
```
# generate migration
rails generate model Otp user:references otp_code:string expires_at:datetime

# verify generated file
db/migrate/20240607150550_create_otps.rb

# Add `null: false` to both :otp_code and datetime

class CreateOtps < ActiveRecord::Migration[7.1]
  def change
    create_table :otps do |t|
      t.references :user, null: false, foreign_key: true
      t.string :otp_code, null: false
      t.datetime :expires_at, null: false

      t.timestamps
    end
  end
end

# run the migration
rails db:migrate

# Questions:
# 1: When we generate a migtation what happens to our database?
# 2: Explain `t.references :user`?
# 3: Explain `t.timestamps`?
# 4: What does `null: false` do in this case?
# 5: What other files were created
```

### Step 4: Create OTP Mutation
```
touch app/graphql/mutations/user_mutations/otp_request.rb

# path: app/graphql/mutations/user_mutations/otp_request.rb

module Mutations
  module UserMutations
    class OtpRequest < Mutations::BaseMutation
      # Define the input arguments for the mutation
      argument :email, String, required: true

      # Define the return types for the mutation
      field :data, Types::UserType, null: true
      field :errors, [String], null: true
      field :message, String, null: false 
      field :http_status, Integer, null: false

      def resolve(email:)
        user = User.find_by(email: email)

        if user
          # Generate random 5-digit OTP
          otp_code = rand.to_s[2..5]
          # Set OTP expiry time to 30 minutes
          expires_at = Time.now + 30.minutes
          # Save OTP to the database
          Otp.create(user: user, otp_code: otp_code, expires_at: expires_at)
          # Send OTP to user email
          UserMailer.otp_email(user, otp_code).deliver_now
          {
            data: user,
            errors: [],
            message: "OTP sent successfully",
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
end

# Questions:
# 1: How do we generate the 4 random numbers?
# 2: When does this code expire?
# 3: How do we calculate the expiration time?
# 4: How do we create the new record for OTP in the database?
# 5: How do we find the user to save this against?
# 6: What happens when user not found?
# 7: What error do we get when we run this mutation query? and why?
# 8: What is used for sending mail to the user?
```

### Step 5: Fixing Unauthorized error
```
# When resetting password, user don't have authorization token, so we need to tell our graphql controller
# to accept any request heading to otpRequest mutation.

# app/controllers/graphql_controller

# update `graphql_authentication_action?` like below

def graphql_authentication_action?
  sign_up_query? || sign_in_query? || otp_query?
end

# add new private method called `otp_query?`

def otp_query?
  params[:query]&.include?('otpRequest')
end

# Questions:
# 1: What does `otp_query?` check?
# 2: What do we achieve by adding `otp_query?` to `graphql_authentication_action?`?
```

### Step 6: Final query request
```
# Visit your graphQL:
http://127.0.0.1:3000/graphiql

mutation {
  otpRequest(input: {
    email: "john16@example.com"
  }) {
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

# Now we have response! and data is stored in our postgres database and we also
# send email to the user with an OTP.
```
