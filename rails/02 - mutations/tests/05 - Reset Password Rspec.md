# Reset Password Testing

### Step 1: Reset Password Spec
```
# generate the spec
rails generate rspec:request auth/reset_password

# replace content with:

require 'rails_helper'

RSpec.describe 'Mutations::UserMutations::ResetPassword', type: :request do
  describe 'resetPassword mutation' do
    let(:user) { create(:user) }
    let(:otp) { create(:otp, user: user, otp_code: '1234', expires_at: 1.hour.from_now) }

    let(:query) do
      <<~GQL
        mutation ResetPassword($otp: String!, $newPassword: String!, $confirmPassword: String!) {
          resetPassword(input: {
            otp: $otp,
            newPassword: $newPassword,
            confirmPassword: $confirmPassword
          }) {
            data {
              id
              email
            }
            errors
            message
            httpStatus
          }
        }
      GQL
    end

    context 'with valid input' do
      let(:variables) do
        {
          otp: otp.otp_code,
          newPassword: 'newSecurepassw@rd3',
          confirmPassword: 'newSecurepassw@rd3'
        }
      end

      it 'resets the password successfully' do
        post graphql_path, params: { query: query, variables: variables }

        json = JSON.parse(response.body)
        data = json['data']['resetPassword']

        expect(data['errors']).to be_empty
        expect(data['message']).to eq('Password reset successfully')
        expect(data['httpStatus']).to eq(200)
        expect(user.reload.valid_password?('newSecurepassw@rd3')).to be true
      end
    end

    context 'with invalid OTP' do
      let(:variables) do
        {
          otp: 'invalidotp',
          newPassword: 'newSecurepassw@rd3',
          confirmPassword: 'newSecurepassw@rd3'
        }
      end

      it 'returns an invalid OTP error' do
        post graphql_path, params: { query: query, variables: variables }

        json = JSON.parse(response.body)
        data = json['data']['resetPassword']

        expect(data['data']).to be_nil
        expect(data['message']).to eq('Invalid OTP')
        expect(data['httpStatus']).to eq(422)
        expect(data['errors']).to include('Invalid OTP')
      end
    end

    context 'when OTP has expired' do
      let(:expired_otp) { create(:otp, user: user, otp_code: '2345', expires_at: 1.hour.ago) }
      let(:variables) do
        {
          otp: expired_otp.otp_code,
          newPassword: 'newSecurepassw@rd3',
          confirmPassword: 'newSecurepassw@rd3'
        }
      end

      it 'returns an OTP expired error' do
        post graphql_path, params: { query: query, variables: variables }

        json = JSON.parse(response.body)
        data = json['data']['resetPassword']

        expect(data['data']).to be_nil
        expect(data['message']).to eq('OTP has expired')
        expect(data['httpStatus']).to eq(422)
        expect(data['errors']).to include('OTP has expired')
      end
    end

    context 'when passwords do not match' do
      let(:variables) do
        {
          otp: otp.otp_code,
          newPassword: 'newSecurepassw@rd3',
          confirmPassword: 'differentpassword'
        }
      end

      it 'returns a passwords do not match error' do
        post graphql_path, params: { query: query, variables: variables }

        json = JSON.parse(response.body)
        data = json['data']['resetPassword']

        expect(data['data']).to be_nil
        expect(data['message']).to eq('Passwords do not match')
        expect(data['httpStatus']).to eq(422)
        expect(data['errors']).to include('Passwords do not match')
      end
    end

    context 'when password update fails' do
      let(:variables) do
        {
          otp: otp.otp_code,
          newPassword: 'short',
          confirmPassword: 'short'
        }
      end

      it 'returns password update failed error' do
        post graphql_path, params: { query: query, variables: variables }

        json = JSON.parse(response.body)
        data = json['data']['resetPassword']

        expect(data['data']).to be_nil
        expect(data['message']).to eq('Password could not be updated')
        expect(data['httpStatus']).to eq(422)
        expect(data['errors']).to_not be_empty
      end
    end

    context 'otp is deleted after password reset' do
      let(:variables) do
        {
          otp: otp.otp_code,
          newPassword: 'newsecurePassw@rd3',
          confirmPassword: 'newsecurePassw@rd3'
        }
      end
    
      it 'deletes the OTP after password reset' do
        expect {
          post graphql_path, params: { query: query, variables: variables }
        }.to change { Otp.exists?(otp.id) }.from(true).to(false)
    
        json = JSON.parse(response.body)
        data = json['data']['resetPassword']
    
        expect(data['errors']).to be_empty
        expect(data['message']).to eq('Password reset successfully')
        expect(data['httpStatus']).to eq(200)
        expect(user.reload.valid_password?('newsecurePassw@rd3')).to be true
      end
    end
  end
end

```