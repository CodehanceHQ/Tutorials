# User Unit Testing

### Step 1: Setup up FactoryBot
```
# create user factory
touch spec/factories/users.rb

# spec/factories/users.rb

FactoryBot.define do
  factory :user do
    full_name { "#{Faker::Name.first_name} #{Faker::Name.last_name}" }
    email { Faker::Internet.email }
    password { "Password@123" }
    password_confirmation { "Password@123" }
    telephone { Faker::PhoneNumber.cell_phone_in_e164 }
  end
end

# Question 1: What is factory bot and how does it work?
# Question 2: What is faker and how does it work?
# Question 3: What other types of faker fields are there?
# Question 4: Does faker give same data each time it is called?
# Question 5: Are we able to pass our own data to Factory bot?
```

### Step 2: Define the Test for the UpdateUser Mutation
```
# Open the existing file `spec/requests/user_mutations_spec.rb` and add the following content:

  describe 'updateUser mutation' do
    let(:user) { create(:user) }
    
    let(:query) do
      <<~GQL
        mutation UpdateUser($id: ID!, $fullName: String!, $email: String!, $telephone: String!) {
          updateUser(input: {
            id: $id,
            fullName: $fullName,
            email: $email,
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
        id: user.id,
        fullName: 'John Updated',
        email: 'john.updated@example.com',
        telephone: '1234567890'
      }
    end

    it 'updates an existing user with valid input' do
      post graphql_path, params: { query: query, variables: variables }

      json = JSON.parse(response.body)
      data = json['data']['updateUser']

      expect(data['errors']).to be_empty
      expect(data['message']).to eq('User updated successfully')
      expect(data['httpStatus']).to eq(200)
      expect(data['data']['fullName']).to eq('John Updated')
      expect(data['data']['email']).to eq('john.updated@example.com')
      expect(data['data']['telephone']).to eq('1234567890')
    end

    it 'returns validation errors for invalid input' do
      invalid_variables = variables.merge(fullName: '', email: 'invalid email', telephone: 'invalid telephone')

      post graphql_path, params: { query: query, variables: invalid_variables }

      json = JSON.parse(response.body)
      data = json['data']['updateUser']

      expect(data['data']).to be_nil
      expect(data['message']).to eq('User update failed')
      expect(data['httpStatus']).to eq(422)
      expect(data['errors']).to include(
        "Email is invalid",
        "Full name can't be blank",
        "Full name is too short (minimum is 4 characters)",
        "Full name is invalid",
        "Telephone is not a number"
      )
    end
  end

# Questions:
# 1. How does FactoryBot help in setting up test data?
# 2. What is the significance of the `variables` hash in these tests?
# 3. How does RSpec differentiate between valid and invalid inputs in these tests?
# 4. What is the role of `graphql_path` in the `post` method?
# 5. Why is it important to validate both successful and unsuccessful updates?
```

### Step 3: Run the Tests
```
# In your terminal, run the tests using the following command:
bundle exec rspec spec/requests/user_mutations_spec.rb
or
bundle exec rspec
```

### Step 4: Run the Tests
```
bundle exec rspec
```

### Step 5: Run the Test Coverage
```
open coverage/index.html

# Question 1: What is your test coverage for user_mutations.rb?
```

### Conclusion:
```
By following these steps, you can effectively test the UpdateUser mutation using RSpec and FactoryBot. 

Testing ensures that the mutation works correctly, validates inputs properly, and handles different scenarios as expected.
```
