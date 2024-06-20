### Step 1: Create spec file
```
mkdir spec/requests/logs
touch spec/requests/logs/log_entries_spec.rb
```

### Step 2: Create content
```
require 'rails_helper'

RSpec.describe "Logs::CreateLog", type: :request do
  let(:user) { create(:user) }
  let(:token) { generate_jwt_token(user) }
  let(:headers) do
    { 'Authorization': "Bearer #{token}" }
  end

  let(:mutation) do
    <<~GQL
      mutation($level: String!, $message: String!, $timestamp: String!, $data: JSON) {
        createLog(input: { level: $level, message: $message, timestamp: $timestamp, data: $data }) {
          status
          message
        }
      }
    GQL
  end

  context 'when log entry is created successfully' do
    it 'returns success status and message' do
      variables = {
        level: "error",
        message: "Failed to sign up2",
        timestamp: "2024-06-19T06:00:45.867Z",
        data: {
          stack: "Error: Simulated signup error\n",
          name: "Error",
          message: "Simulated signup error"
        }
      }

      post graphql_path, params: { query: mutation, variables: variables }, headers: headers

      json_response = JSON.parse(response.body)
      data = json_response['data']['createLog']

      expect(data['status']).to eq('success')
      expect(data['message']).to eq('Log entry created')
    end
  end

  context 'when log entry creation fails' do
    it 'returns error status and message' do
      allow_any_instance_of(LogEntry).to receive(:save).and_return(false)
      allow_any_instance_of(LogEntry).to receive_message_chain(:errors, :full_messages).and_return(['Error message'])

      variables = {
        level: "error",
        message: "Failed to sign up2",
        timestamp: "2024-06-19T06:00:45.867Z",
        data: {
          stack: "Error: Simulated signup error\n",
          name: "Error",
          message: "Simulated signup error"
        }
      }

      post graphql_path, params: { query: mutation, variables: variables }, headers: headers

      json_response = JSON.parse(response.body)
      data = json_response['data']['createLog']

      expect(data['status']).to eq('error')
      expect(data['message']).to eq('Error message')
    end
  end

  context 'when authorization header is not set' do
    it 'creates the log entry successfully' do
      variables = {
        level: "error",
        message: "Failed to sign up2",
        timestamp: "2024-06-19T06:00:45.867Z",
        data: {
          stack: "Error: Simulated signup error\n",
          name: "Error",
          message: "Simulated signup error"
        }
      }

      post graphql_path, params: { query: mutation, variables: variables }

      json_response = JSON.parse(response.body)
      data = json_response['data']['createLog']

      expect(data['status']).to eq('success')
      expect(data['message']).to eq('Log entry created')
    end
  end
end

# Questions:
# 1: What contexts are we testing?
# 2: Explain the test `returns error status and message`?
```