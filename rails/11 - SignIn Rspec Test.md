# User Unit Testing

### Step 1: Define the Test for the SignIn Mutation
```
# Open the existing file `spec/requests/user_mutations_spec.rb` and add the following content:

  describe 'signIn mutation' do
    let(:user) { create(:user, password: 'Password@123') }
    
    let(:query) do
      <<~GQL
        mutation SignIn($email: String!, $password: String!) {
          signIn(input: {
            email: $email,
            password: $password
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
        email: user.email,
        password: 'Password@123'
      }
    end

    it 'authenticates an existing user with valid credentials' do
      post graphql_path, params: { query: query, variables: variables }

      json = JSON.parse(response.body)
      data = json['data']['signIn']

      expect(data['errors']).to be_empty
      expect(data['message']).to eq('User signed in successfully')
      expect(data['httpStatus']).to eq(200)
      expect(data['data']['fullName']).to eq(user.full_name)
      expect(data['data']['email']).to eq(user.email)
      expect(data['data']['telephone']).to eq(user.telephone)
    end

    it 'returns authentication errors for invalid credentials' do
      invalid_variables = variables.merge(password: 'WrongPassword')

      post graphql_path, params: { query: query, variables: invalid_variables }

      json = JSON.parse(response.body)
      data = json['data']['signIn']

      expect(data['data']).to be_nil
      expect(data['message']).to eq('User sign in failed')
      expect(data['httpStatus']).to eq(401)
      expect(data['errors']).to include('Invalid email or password')
    end
  end

# Questions:
# 1. How does FactoryBot simplify test data creation?
# 2. How does this use user factory `create(:user, password: 'Password@123')`
# 3. Explain what is happening here `let(:user)`
```

### Step 2: Run the Tests
```
# In your terminal, run the tests using the following command:
bundle exec rspec spec/requests/user_mutations_spec.rb
or
bundle exec rspec
```

### Step 3: Run the Test Coverage
```
open coverage/index.html

# Questions:
# 1. What is your test coverage for user_mutations.rb?
```

### Conclusion:
```
# By following these steps, you can effectively test the SignIn mutation using RSpec and FactoryBot. 

# Testing ensures that the mutation works correctly, validates inputs properly, and handles different scenarios as expected.
```