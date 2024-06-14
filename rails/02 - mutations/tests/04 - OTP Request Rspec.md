### Step 1: Setup otp factory
```
# spec/factories/otps.rb
FactoryBot.define do
  factory :otp do
    association :user
    otp_code { rand.to_s[2..5] }
    expires_at { Time.now + 30.minutes }
  end
end
```

### Step 2: Generate otp spec
```
rails generate rspec:request auth/otp_request
```

### Step 3: Update otp request spec
```
# spec/requests/auth/otp_requests_spec.rb

require 'rails_helper'

RSpec.describe "Auth::OtpRequests", type: :request do
  describe "otp_requests mutation" do
    let(:user) { create(:user, email: 'john16@example.com') }

    let(:query) do
      <<~GQL
        mutation otpRequest($email: String!) {
          otpRequest(input: { email: $email }) {
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

    it 'sends an OTP to the user and returns the user data' do
      expect {
        post graphql_path, params: { query: query, variables: { email: user.email } }
      }.to change { ActionMailer::Base.deliveries.count }.by(1)
  
      json = JSON.parse(response.body)
      otp_response = json['data']['otpRequest']
  
      expect(otp_response['data']['email']).to eq(user.email)
      expect(otp_response['message']).to eq('OTP sent successfully')
      expect(otp_response['errors']).to be_empty
      expect(otp_response['httpStatus']).to eq(200)
    end
  
    it 'returns an error if the user is not found' do
      expect {
        post graphql_path, params: { query: query, variables: { email: "invalid@example.com" } }
      }.to change { ActionMailer::Base.deliveries.count }.by(0)
  
      json = JSON.parse(response.body)
      otp_response = json['data']['otpRequest']
  
      expect(otp_response['data']).to be_nil
      expect(otp_response['message']).to eq('User not found')
      expect(otp_response['errors']).to include('User not found')
      expect(otp_response['httpStatus']).to eq(404)
    end
  end
end
```