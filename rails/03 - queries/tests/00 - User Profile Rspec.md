## User Profile Spec

### Step 1: Create spec
```
rails generate rspec:request auth/user_profile
```

### Step 2: Replace the spec file
```
require 'rails_helper'

RSpec.describe "Auth::UserProfiles", type: :request do
  describe "query user_profiles" do
    let(:user) { create(:user) }
    let(:token) { generate_jwt_token(user) }
    let(:headers) { { 'Authorization' => token } }

    let(:query) do
      <<~GQL
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
      GQL
    end

    context 'when the user is authenticated' do
      it 'returns the user profile' do
        post graphql_path, params: { query: query }, headers: headers

        json = JSON.parse(response.body)
        data = json['data']['userProfile']
  
        expect(data).to include(
          'data' => {
            'id' => user.id.to_s,
            'fullName' => user.full_name,
            'email' => user.email,
            'telephone' => user.telephone
          },
          'errors' => [],
          'message' => 'Profile fetched successfully',
          'httpStatus' => 200
        )
      end
  
      it 'matches the JWT user id with the returned value' do
        post graphql_path, params: { query: query }, headers: headers

        json = JSON.parse(response.body)
        data = json['data']['userProfile']

        returned_id = data['data']['id']
  
        expect(returned_id).to eq(user.id.to_s)
      end
  
      it 'returns the correct format' do
        post graphql_path, params: { query: query }, headers: headers

        json = JSON.parse(response.body)
        data = json['data']['userProfile']

        expect(data).to have_key('data')
        expect(data).to have_key('errors')
        expect(data).to have_key('message')
        expect(data).to have_key('httpStatus')
      end
    end
  
    context 'when the user is not authenticated' do
      let(:headers) { {} }
  
      it 'returns an error' do
        post graphql_path, params: { query: query }, headers: headers

        json = JSON.parse(response.body)
  
        expect(json).to include(
          'data' => nil,
          'errors' => ['Unauthorized'],
          'message' => 'Unauthorized',
          'httpStatus' => 401
        )
      end
    end
  end
end
```