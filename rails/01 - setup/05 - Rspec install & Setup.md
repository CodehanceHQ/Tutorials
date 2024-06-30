## Setting up RSpec

### Step 1: Understanding RSpec

```
# Questions:
# 1. What is RSpec and what are its primary uses in a Rails application?
# 2. How does RSpec differ from other testing frameworks available for Ruby?
# 3. What is the purpose of using test doubles (mocks and stubs) in RSpec?
# 4. Explain the role of FactoryBot in an RSpec test suite.
# 5. How can you use RSpec to write both unit tests and integration tests?
```

### Step 2: Add Test Gems
```
# Test gems
group :development, :test do
  gem 'rspec-rails', '~> 5.0'
  gem 'factory_bot_rails'
  gem 'database_cleaner-active_record'
  gem 'faker'
  gem 'simplecov', require: false
end

# Questions:
# 1. What is the purpose of adding the `rspec-rails` gem?
# 2. How does `factory_bot_rails` assist in testing?
# 3. What is `database_cleaner-active_record` used for?
# 4. Why is `faker` included in the test group?
# 5. What does the `simplecov` gem provide for testing?
```

### Step 3: Install the Gems

```
# Install the gems
bundle install

# Questions:
# 1. What does the command `bundle install` do?
# 2. How does Bundler ensure that the correct versions of gems are installed?
# 3. What file does Bundler use to manage gem dependencies?
# 4. What could go wrong if `bundle install` is not run after adding new gems?
```

### Step 4: Run the RSpec Installation Generator
```
# Run the RSpec installation generator
rails generate rspec:install

# Questions:
# 1. What does the command `rails generate rspec:install` do?
# 2. What files and configurations are set up by this generator?
# 3. How can you verify that RSpec was installed correctly?
# 4. What are some best practices for organizing your specs with RSpec?
```

### Step 5: Update `spec/spec_helper.rb`

```
# spec/spec_helper.rb
require 'simplecov'
SimpleCov.start 'rails' do
  add_filter '/bin/'              # Exclude bin directory
  add_filter '/db/'               # Exclude db directory
  add_filter '/spec/'             # Exclude spec directory
  add_filter '/channels/'         # Exclude channels directory
  add_filter '/jobs/'             # Exclude jobs directory

  add_filter '/graphql/resolvers/base_resolver.rb'  # Exclude specific file
  add_filter '/graphql/types/base_enum.rb'          # Exclude specific file
  add_filter '/graphql/types/base_interface.rb'     # Exclude specific file
  add_filter '/graphql/types/base_scalar.rb'        # Exclude specific file
  add_filter '/graphql/types/base_union.rb'         # Exclude specific file
  add_filter '/graphql/types/node_type.rb'          # Exclude specific file
end

RSpec.configure do |config|
  # Your existing configuration here
end

# Questions:
# 1. Why is the `simplecov` gem used in this setup?
# 2. Explain the purpose of `add_filter` in the `SimpleCov.start` block.
# 3. What would happen if the `SimpleCov.start` method is not called at the beginning of the spec_helper file?
# 4. How does excluding certain directories/files from SimpleCov reports benefit the development process?
# 5. Why is it important for SimpleCov to start before any application code is loaded?
```

### Step 6: Replace Rails Helper
```
# spec/rails_helper.rb
require 'spec_helper'
ENV['RAILS_ENV'] ||= 'test'
require File.expand_path('../config/environment', __dir__)
abort("The Rails environment is running in production mode!") if Rails.env.production?

require 'rspec/rails'
require 'factory_bot_rails'
require 'database_cleaner/active_record'
require 'faker'

# Add additional requires below this line. Rails is not loaded until this point!

# Load support files
Dir[Rails.root.join('spec/support/**/*.rb')].each { |f| require f }

# Checks for pending migrations and applies them before tests are run.
# If you are not using ActiveRecord, you can remove this line.
begin
  ActiveRecord::Migration.maintain_test_schema!
rescue ActiveRecord::PendingMigrationError => e
  puts e.to_s.strip
  exit 1
end

RSpec.configure do |config|
  # Use FactoryBot syntax methods
  config.include FactoryBot::Syntax::Methods

  # Use Database Cleaner
  config.before(:suite) do
    DatabaseCleaner.clean_with(:truncation)
  end

  config.before(:each) do
    DatabaseCleaner.strategy = :transaction
  end

  config.before(:each, js: true) do
    DatabaseCleaner.strategy = :truncation
  end

  config.before(:each) do
    DatabaseCleaner.start
  end

  config.after(:each) do
    DatabaseCleaner.clean
  end

  config.fixture_paths = [
    Rails.root.join('spec/fixtures')
  ]

  # Include Devise test helpers for request specs
  config.include Devise::Test::IntegrationHelpers, type: :request

  # Include Warden test helpers for request specs
  config.include Warden::Test::Helpers
  config.after(:each) { Warden.test_reset! }

  # Include JWT helper
  config.include JwtHelper

  # Include GraphQL helper
  config.include GraphqlHelper, type: :request

  # Use transactional fixtures
  config.use_transactional_fixtures = false

  # Infer spec type from file location
  config.infer_spec_type_from_file_location!

  # Filter lines from Rails gems in backtraces
  config.filter_rails_from_backtrace!

  # You can add additional configurations below this line
end

# Questions:
# 1. What is the purpose of `rails_helper.rb` in the context of RSpec?
# 2. Why do we include FactoryBot syntax methods in the configuration?
# 3. Explain the role of `DatabaseCleaner` in the test setup.
# 4. How do Devise test helpers assist in writing tests?
# 5. What is the significance of setting `config.use_transactional_fixtures` to false?
```

### Step 7: Create Directories for Specs

```
# Create directories for your specs
mkdir -p spec/models &&
mkdir -p spec/controllers &&
mkdir -p spec/requests &&
mkdir -p spec/support &&
mkdir -p spec/factories

# Questions:
# 1. Why is it necessary to create separate directories for models, controllers, and request specs?
# 2. How does structuring your specs into directories improve maintainability?
# 3. What would be the impact of not creating the necessary spec directories?
# 4. What best practices should you follow when organizing your spec directories?
```

### Step 8: Ensure config/database.yml is Configured for the Test Environment
```
# Ensure your config/database.yml is configured
test:
  <<: *default
  database: your_app_test

# Questions:
# 1. What is the purpose of the test database configuration in `database.yml`?
# 2. How does this configuration affect your test environment?
# 3. What could happen if the test database configuration is incorrect or missing?
# 4. How do you verify that your test database configuration is set up correctly?
```

### Step 9: Configure Application to Use RSpec

```
# Don't replace YourAppName, keep yours!

# config/application.rb
module YourAppName
  class Application < Rails::Application
    # Other configurations...

    # Use RSpec for testing
    config.generators do |g|
      g.test_framework :rspec,
        fixtures: true,
        helper_specs: true,
        routing_specs: true,
        request_specs: true,
        controller_specs: true
      g.fixture_replacement :factory_bot, dir: "spec/factories"
    end
  end
end

# Questions:
# 1. Why do we specify the test framework configuration in `application.rb`?
# 2. How does this configuration benefit the development process?
# 3. What are the implications of not configuring the test framework in `application.rb`?
# 4. How does specifying the fixture replacement with `factory_bot` help in testing?
```

### Step 10: Add GraphQL Helper
```
# spec/support/graphql_helper.rb
# replace YourAppNameSchema to yours located in app/graphql/xxx_schema.rb
touch spec/support/graphql_helper.rb

module GraphqlHelper
  def graphql_query(query, variables: {}, context: {})
    YourAppNameSchema.execute(query, variables: variables, context: context)
  end
end

# Questions:
# 1. What is the purpose of the GraphQL helper module?
# 2. How does the `graphql_query` method simplify writing tests for GraphQL queries?
# 3. Explain the significance of replacing `YourAppNameSchema` with your actual schema name.
# 4. How would you use the `graphql_query` method in a test case?
```

### Step 11: Add JWT Helper
```
# spec/support/jwt_helper.rb
# create jwt_helper.rb
touch spec/support/jwt_helper.rb

module JwtHelper
  def generate_jwt_token(user)
    payload = {
      sub: user.id,
      jti: SecureRandom.uuid,
      exp: 24.hours.from_now.to_i
    }
    JWT.encode(payload, Rails.application.credentials.jwt_secret_key)
  end

  def decode_jwt_token(token)
    JWT.decode(token, Rails.application.credentials.jwt_secret_key).first
  end

  def encode_jwt_token(payload)
    JWT.encode(payload, Rails.application.credentials.jwt_secret_key)
  end
end

# Questions:
# 1. What is the role of the JWT helper module in testing?
# 2. Explain how the `generate_jwt_token` method works.
# 3. How would you use the `decode_jwt_token` method in your tests?
# 4. What is the significance of using `Rails.application.credentials.jwt_secret_key`?
```

### Step 12: Run RSpec to See `0 examples, 0 failures`
```
# Run RSpec
rspec

# Questions:
# 1. What does the output `0 examples, 0 failures` indicate after running RSpec?
# 2. How would you add your first test case to ensure RSpec is working correctly?
# 3. Why is it important to run your test suite regularly during development?
# 4. What steps would you take if your tests fail unexpectedly?
```

### Step 13: View test coverage
```
# enter in your terminal
open coverage/index.html

# Questions:
# 1. What does the `coverage/index.html` file represent in the context of test coverage?
# 2. How can you use the information displayed in the coverage report to improve your test suite?
# 3. What metrics are typically included in a test coverage report, and what do they indicate?
# 4. Why is it important to regularly check and maintain high test coverage for your codebase?
```

### Conclusion

```
In this guide, we've covered the essential steps to set up and configure RSpec in a Rails application. By following these steps, you now have a robust testing framework that allows you to write and run tests efficiently.

Understanding the purpose and functionality of each configuration and helper ensures that your test suite is well-organized and maintainable.

Right now we have zero test coverage, we aim to change that, regularly running your tests and maintaining high test coverage with tools like SimpleCov will help you catch bugs early and ensure the reliability of your application. Happy testing!
```
