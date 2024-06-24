## GraphQL OTP ResetPassword Mutation

### Step 1: Send OTP Request Mutation
```
# email below has to be a user already registered but 
# forgot their login details.

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
```

### Step 2: Send ResetPassword Mutation
```
# verify that the otp code is valid from database

# Visit your graphQL:
http://127.0.0.1:3000/graphiql

mutation {
  resetPassword(input: {
    otp: "6562",
    newPassword: "newPassw@ord1",
    confirmPassword: "newPassw@ord1"
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

# No header information required
```

### Step 3: Create mutatble type
```
# app/graphql/types/mutation_type.rb

module Types
  class MutationType < Types::BaseObject
    # leave the rest
    field  :resetPassword, mutation: Mutations::UserMutations::ResetPassword 
  end
end
```

### Step 4: Create ResetPassword mutatble
```
touch app/graphql/mutations/user_mutations/reset_password.rb

# path: app/graphql/mutations/user_mutations/reset_password.rb

module Mutations
  module UserMutations
    class ResetPassword < BaseMutation
      # Define the input arguments for the mutation
      argument :otp, String, required: true
      argument :new_password, String, required: true
      argument :confirm_password, String, required: true

      # Define the return types for the mutation
      field :data, Types::UserType, null: true
      field :errors, [String], null: true
      field :message, String, null: false 
      field :http_status, Integer, null: false

      # Define the resolve method
      def resolve(otp:, new_password:, confirm_password:)
        # Find the OTP
        user_otp = Otp.find_by(otp_code: otp)
        
        unless user_otp
          return response(nil, ["Invalid OTP"], "Invalid OTP", 422)
        end

        # Check if the OTP has expired
        if user_otp.expires_at < Time.current
          return {
            data: nil,
            errors: ["OTP has expired"],
            message: "OTP has expired",
            http_status: 422
          }
        end

        # Check passwords match
        if new_password != confirm_password
          return {
            data: nil,
            errors: ["Passwords do not match"],
            message: "Passwords do not match",
            http_status: 422
          }
        end

        # Find the user
        user = user_otp.user

        # Update the user's password
        if user.update(password: new_password)
          # delete the OTP after the password has been updated
          user_otp.destroy

          # Return the user
          {
            data: user,
            errors: [],
            message: "Password reset successfully",
            http_status: 200
          }
        else 
          # Return an error if the password could not be updated
          {
            data: nil,
            errors: user.errors.full_messages,
            message: "Password could not be updated",
            http_status: 422
          }
        end
      end

      private

      def response(data, errors, message, http_status)
        {
          data: data,
          errors: errors,
          message: message,
          http_status: http_status
        }
      end
    end
  end 
end 
```

### Step 5: Send ResetPassword Mutation Again
```
# Visit your graphQL:
http://127.0.0.1:3000/graphiql

mutation {
  resetPassword(input: {
    otp: "6562",
    newPassword: "newPassw@ord1",
    confirmPassword: "newPassw@ord1"
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
```