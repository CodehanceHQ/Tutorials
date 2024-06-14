# Logout Unit Testing

### Step 1: Logout Spec
```
# generate the spec
rails generate rspec:request auth/logout

# replace content with:

require 'rails_helper'

RSpec.describe "Auth::Logouts", type: :request do
  describe 'Logout Mutation' do
    # Create a user and generate a JWT token for authentication
    let(:user) { create(:user) }
    let(:token) { generate_jwt_token(user) }
  
    # Define the GraphQL logout mutation query
    let(:query) do
      <<~GQL
        mutation {
          logout(input: {}) {
            data {
              id
              fullName
              email
              telephone
            }
            message
            errors
            httpStatus
          }
        }
      GQL
    end
  
    describe 'Logging out a user' do
      context 'when logging out for the first time' do
        it 'logs out successfully' do
          # Send the logout request with the JWT token
          headers = { 'Authorization' => "Bearer #{token}" }
          post graphql_path, params: { query: query }, headers: headers
  
          # Parse the JSON response
          json = JSON.parse(response.body)
          data = json['data']['logout']
          
          # Check that the response includes the correct user data
          expect(data['data']).to include(
            'id' => user.id.to_s,
            'fullName' => user.full_name,
            'email' => user.email,
            'telephone' => user.telephone
          )
          expect(data['errors']).to be_empty
          # Check that the response includes a success message
          expect(data['message']).to eq('Successfully logged out')
          # Check that there are no errors in the response
          expect(data['errors']).to be_empty
        end
      end
  
      context 'when logging out twice' do
        it 'returns already logged out error' do
          # Send the logout request with the JWT token
          headers = { 'Authorization' => "Bearer #{token}" }
          post graphql_path, params: { query: query }, headers: headers

          # Send the logout request again with the same JWT token
          post graphql_path, params: { query: query }, headers: headers
  
          # Parse the JSON response
          json = JSON.parse(response.body)
          data = json['data']['logout']
          
          # Check that the response includes an "Already logged out" message
          expect(data['message']).to eq('Already logged out')
          # Check that the response includes the appropriate error message
          expect(data['errors']).to include('You are already logged out')
        end
      end
  
      context 'response format' do
        it 'returns the correct format' do
          # Send the logout request with the JWT token
          headers = { 'Authorization' => "Bearer #{token}" }
          post graphql_path, params: { query: query }, headers: headers
  
          # Parse the JSON response
          json = JSON.parse(response.body)
          data = json['data']['logout']
          
          # Check that the response includes the user data in the correct format
          expect(data).to include(
            'data' => a_hash_including(
              'id' => be_present,
              'fullName' => be_present,
              'email' => be_present,
              'telephone' => be_present
            ),
            # Check that the response includes a message field
            'message' => be_present,
            # Check that the response includes an errors array
            'errors' => be_an(Array),
            # Check that the response includes httpStatus field
            'httpStatus' => 200 # Ok
          )
        end
      end
    end
  end  
end
```

### Step 2: Run the spec
```
# run:
rspec spec/requests/auth/logouts_spec.rb
```