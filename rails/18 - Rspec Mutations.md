### Writing mutation tests

```
# Step 1: Set up user mutations spec
# Questions:
# 1. Why is it necessary to create directories for your GraphQL mutation specs?
# 2. Explain the purpose of the `let(:query)` block in the RSpec test.
# 3. How does the use of `variables` enhance the flexibility of the test?
# 4. Why convert this to json `variables.to_json`?

# spec/requests/graphql/mutations/user_mutations_spec.rb

# create user_mutations_spec
mkdir -p spec/requests/graphql/mutations
touch spec/requests/graphql/mutations/user_mutations_spec.rb

require 'rails_helper'
require 'jwt'

RSpec.describe 'signUp Mutation', type: :request do
  let(:query) do
    <<~GQL
      mutation($input: SignUpInput!) {
        signUp(input: $input) {
          message
          user {
            id
            name
            email
          }
        }
      }
    GQL
  end

  it 'creates a new user' do
    variables = {
      input: {
        name: 'Dave Doe',
        email: 'john@example.com',
        password: 'password123'
      }
    }

    post '/graphql', params: { query: query, variables: variables.to_json }

    expect(response).to have_http_status(:ok)

    json = JSON.parse(response.body)
    data = json['data']['signUp']

    expect(data['message']).to eq("User created successfully")
    expect(data['user']).to include(
      "email" => "john@example.com"
    )

    # Verify that the user is created in the database
    user = User.find_by(email: 'john@example.com')
    expect(user).not_to be_nil
    expect(user.name).to eq('Dave Doe')
  end
end

# Step 2: Run spec
# Questions:
# 1. What does running `rspec` accomplish at this stage?
# 2. How do you interpret the results of the test run?
# 3. What steps would you take if the test fails?

rspec

# Step 3: Set up SignIn mutation test
# Questions:
# 1. What is the role of factories in setting up tests?
# 2. Why is it important to log the response for debugging?
# 3. How does testing for invalid credentials improve the robustness of your application?
# What is a faker gem? and how how `{ Faker::Name.name }` work?

# spec/factories/users.rb

# create users factory
touch spec/factories/users.rb

FactoryBot.define do
  factory :user do
    name { Faker::Name.name }
    email { Faker::Internet.email }
    password { 'password123' }
  end
end

# spec/requests/graphql/mutations/user_mutations_spec.rb

# Update add following Spec after

RSpec.describe 'SignIn Mutation', type: :request do
  let!(:user) { create(:user, email: 'john@example.com', password: 'password123') }

  let(:query) do
    <<~GQL
      mutation($input: SignInInput!) {
        signIn(input: $input) {
          message
          user {
            id
            name
            email
          }
          token
        }
      }
    GQL
  end

  it 'signs in an existing user' do
    variables = {
      input: {
        email: 'john@example.com',
        password: 'password123'
      }
    }

    post '/graphql', params: { query: query, variables: variables.to_json }

    expect(response).to have_http_status(:ok)

    json = JSON.parse(response.body)
    puts json # Logging response for debugging
    data = json['data']['signIn']

    expect(data['message']).to eq("Signed in successfully")
    expect(data['user']).to include(
      'email' => 'john@example.com'
    )
    expect(data['token']).not_to be_nil
  end

  context 'when the credentials are invalid' do
    it 'returns an error message' do
      variables = {
        input: {
          email: 'john@example.com',
          password: 'wrongpassword'
        }
      }

      post '/graphql', params: { query: query, variables: variables }.to_json, headers: { "Content-Type" => "application/json" }

      expect(response).to have_http_status(:ok)

      json = JSON.parse(response.body)
      data = json['data']['signIn']

      expect(data['message']).to eq("Invalid email or password")
      expect(data['user']).to be_nil
      expect(data['token']).to be_nil
    end
  end
end

# Step 3: Run spec
# Questions:
# 1. What does the output of the test tell you about the SignIn mutation?
# 2. Why is it important to test both successful and unsuccessful sign-in attempts?
# 3. How would you modify the test if the authentication method changes?

rspec

# Step 4: Update User mutation test
# Questions:
# 1. Why do we use JWT tokens in these tests?
# 2. How does setting headers with the token ensure security in your tests?
# 3. What are the benefits of testing user updates via GraphQL mutations?

# spec/requests/graphql/mutations/user_mutations_spec.rb

RSpec.describe 'UpdateUser Mutation', type: :request do
  let(:user) { create(:user) }
  let(:token) { generate_jwt_token(user) }
  let(:headers) { { "Authorization" => "Bearer #{token}", "Content-Type" => "application/json" } }

  let(:query) do
    <<~GQL
      mutation($input: UpdateUserInput!) {
        updateUser(input: $input) {
          message
          user {
            id
            name
            email
          }
        }
      }
    GQL
  end

  it 'updates an existing user' do
    variables = {
      input: {
        id: user.id,
        name: 'Jane Doe',
        email: 'jane@example.com'
      }
    }

    post '/graphql',
         params: { query: query, variables: variables }.to_json,
         headers: headers

    expect(response).to have_http_status(:ok)

    json = JSON.parse(response.body)
    data = json['data']['updateUser']

    expect(data['message']).to eq("User updated successfully")
    expect(data['user']).to include(
      'id' => user.id.to_s,
      'name' => 'Jane Doe',
      'email' => 'jane@example.com'
    )

    user.reload
    expect(user.name).to eq('Jane Doe')
    expect(user.email).to eq('jane@example.com')
  end
end

# Step 5: Run spec
# Questions:
# 1. How do you verify that the user data has been correctly updated?
# 2. What is the significance of using `user.reload` in the test?
# 3. How would you handle testing for multiple updates in the same mutation?

rspec

# Step 6: Delete User mutation test
# Questions:
# 1. Why is it important to test the deletion of a user?
# 2. How does the test ensure that the user is properly deleted from the database?
# 3. What potential issues could arise if the delete user test is not implemented correctly?

RSpec.describe 'DeleteUser Mutation', type: :request do
  let(:user) { create(:user) }
  let(:token) { generate_jwt_token(user) }
  let(:headers) { { "Authorization" => "Bearer #{token}", "Content-Type" => "application/json" } }

  let(:query) do
    <<~GQL
      mutation($input: DeleteUserInput!) {
        deleteUser(input: $input) {
          id
          email
          name
        }
      }
    GQL
  end

  it 'deletes an existing user' do
    variables = {
      input: {
        id: user.id
      }
    }

    post '/graphql',
         params: { query: query, variables: variables }.to_json,
         headers: headers

    expect(response).to have_http_status(:ok)

    json = JSON.parse(response.body)
    data = json['data']['deleteUser']

    # Deleted user should be returned
    expect(data).to include(
      'id' => user.id.to_s,
      'email' => user.email,
      'name' => user.name
    )

    # Ensure the user is deleted
    expect(User.find_by(id: user.id)).to be_nil
  end
end

# Step 7: Run spec
# Questions:
# 1. What is the expected outcome when the DeleteUser mutation test passes?
# 2. How do you handle scenarios where the user cannot be deleted?
# 3. Explain the importance of cleaning up test data after each test run.

rspec

# Step 8: Reset Password mutation test
# Questions:
# 1. What is the purpose of the reset password mutation?
# 2. How do you ensure that the reset password instructions are sent to the correct email?
# 3. What are the implications of not handling the case where the user does not exist?

RSpec.describe 'ResetPassword Mutation', type: :request do
  let!(:user) { create(:user) }
  let(:token) { generate_jwt_token(user) }
  let(:headers) { { "Authorization" => "Bearer #{token}", "Content-Type" => "application/json" } }

  let(:query) do
    <<~GQL
      mutation($input: ResetPasswordInput!) {
        resetPassword(input: $input) {
          message
          user {
            id
            email
            name
          }
        }
      }
    GQL
  end

  before(:each) do
    Rails.application.routes.default_url_options[:host] = 'localhost:3000'
  end

  context 'when the user exists' do
    it 'sends reset password instructions and returns a success message' do
      variables = {
        input: {
          email: user.email
        }
      }

      post '/graphql',
           params: { query: query, variables: variables }.to_json,
           headers: headers

      expect(response).to have_http_status(:ok)

      json = JSON.parse(response.body)
      data = json['data']['resetPassword']

      expect(data['message']).to eq("Reset password instructions have been sent to #{user.email}")
      expect(data['user']).to include(
        'id' => user.id.to_s,
        'email' => user.email,
        'name' => user.name
      )

      # Check if reset password instructions were sent
      expect(ActionMailer::Base.deliveries.last.to).to include(user.email)
    end
  end

  context 'when the user does not exist' do
    it 'returns a user not found message' do
      variables = {
        input: {
          email: 'nonexistent@example.com'
        }
      }

      post '/graphql',
           params: { query: query, variables: variables }.to_json,
           headers: headers

      expect(response).to have_http_status(:ok)

      json = JSON.parse(response.body)
      data = json['data']['resetPassword']

      expect(data['message']).to eq('User not found')
      expect(data['user']).to be_nil
    end
  end
end

# Step 9: Run spec
# Questions:
# 1. What does it mean if the ResetPassword mutation test passes?
# 2. How can you verify that the email delivery was successful?
# 3. What additional edge cases should you consider for the ResetPassword mutation?

rspec
```
