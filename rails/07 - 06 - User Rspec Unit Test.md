# User Unit Testing

### Step 1: Generate the Request Spec File
```
# Step-by-Step Guide for Testing the UserMutation with RSpec

# In your terminal, run the following command to generate the request spec file:
rails generate rspec:request user_mutations

# Questions:
# 1. What command do you use to generate an RSpec request spec file?
# 2. Where is the generated request spec file located?
# 3. Why do we generate a request spec for testing GraphQL mutations?
# 4. What type of tests are typically written in request specs?
```

### Step 2: Define the Test for the SignUp Mutation
```
# Open the generated file `spec/requests/user_mutations_spec.rb` and replace with content:

require 'rails_helper'

RSpec.describe 'UserMutations', type: :request do
  describe 'signUp mutation' do
    let(:query) do
      <<~GQL
        mutation SignUp($fullName: String!, $email: String!, $password: String!, $confirmPassword: String!, $telephone: String!) {
          signUp(input: {
            fullName: $fullName,
            email: $email,
            password: $password,
            confirmPassword: $confirmPassword,
            telephone: $telephone
          }) {
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
      GQL
    end

    let(:variables) do
      {
        fullName: 'John Doe',
        email: 'john.doe@example.com',
        password: 'Password@123',
        confirmPassword: 'Password@123',
        telephone: '1234567890'
      }
    end

    it 'creates a new user with valid input' do
      post graphql_path, params: { query: query, variables: variables }

      json = JSON.parse(response.body)
      data = json['data']['signUp']

      expect(data['errors']).to be_empty
      expect(data['message']).to eq('User created successfully')
      expect(data['httpStatus']).to eq(200)
      expect(data['data']['fullName']).to eq('John Doe')
      expect(data['data']['email']).to eq('john.doe@example.com')
      expect(data['data']['telephone']).to eq('1234567890')
    end

    it 'returns validation errors for empty input' do
      invalid_variables = variables.merge(fullName: '', email: '', password: '', confirmPassword: '', telephone: '')

      post graphql_path, params: { query: query, variables: invalid_variables }

      json = JSON.parse(response.body)
      data = json['data']['signUp']

      expect(data['data']).to be_nil
      expect(data['message']).to eq('User creation failed')
      expect(data['httpStatus']).to eq(422)
      expect(data['errors']).to include(
        "Email can't be blank",
        "Password can't be blank",
        "Full name can't be blank",
        "Full name is too short (minimum is 4 characters)",
        "Full name is invalid",
        "Telephone can't be blank",
        "Telephone is not a number",
        "Password can't be blank",
        "Password is too short (minimum is 8 characters)",
        "Password requires one uppercase letter and one special character"
      )
    end
  end
end

# Questions:
# 1. What is the purpose of the `let` block in RSpec?
# 2. What does the `post` method do in the context of this test?
# 3. What does it mean to parse JSON response in the test?
# 4. Why is it important to test both valid and invalid inputs?
# 5. Why did we `require 'rails_helper'`?
# 6. What is a red - green testing?
```

### Step 3: Run the Tests
```
# In your terminal, run the tests using the following command:
bundle exec rspec spec/requests/user_mutations_spec.rb
or
bundle exec rspec

# Questions:
# 1. What command do you use to run the RSpec tests?
# 2. What do you understand by reading our failing tests `red`?
# 3. Why is it important to ensure tests pass before deploying code?
```

### Step 4: Fixing test
```
# Update user model:

# app/models/user.rb

class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable,
         :jwt_authenticatable, jwt_revocation_strategy: JwtDenylist

  validates :full_name, presence: true, length: { minimum: 4 }
  validates :full_name, format: { with: /\A[a-zA-Z]+\s[a-zA-Z]+\z/ }
  validates :telephone, presence: true, numericality: true
  validates :password, presence: true, length: { minimum: 8 }, on: :create
  validate :password_format, on: :create

  private

  def password_format
    return if password.match?(/^(?=.*[A-Z])(?=.*[!@#\$%\^&\*])(?=.{8,})/)

    errors.add(:password, 'requires one uppercase letter and one special character')
  end
end

# Question 1: What is regex and how did we apply it?
# Question 2: What are custom validations?
# Question 3: How did we apply custom validation above?
# Question 4: Why did we add `on: :create` to password and password_format?
```

### Step 5: Run the Tests
```
bundle exec rspec
```

### Step 6: Run the Test Coverage
```
open coverage/index.html

# Question 1: What did you learn from the test coverage?
# Question 2: Can you view lines of un tested bits?
```

### Conclusion:
```
By following these steps, you can effectively test the SignUp mutation using RSpec and FactoryBot. 

Testing ensures that the mutation works correctly, validates inputs properly, and handles different scenarios as expected.
```