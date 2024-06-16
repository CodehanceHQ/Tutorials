# Rails API

### This guide will walk you through setting up a new Rails API


### Rails API Understanding

```
# Questions:
# 1. What is a Rails API-only application and how does it differ from a standard Rails application?
# 2. What are some of the advantages of using a Rails API-only application for backend development?
# 3. What are some common strategies for authentication in rails?
# 4. What CORS (Cross-Origin Resource Sharing) in a Rails API-only application?
```

### Step 1: Create a New Rails API-Only Application
```
# Create a new Rails API-only application with PostgreSQL
rails new my_api_app --api -d postgresql --skip-test

# Questions:
# 1. What command creates a Rails API-only application with PostgreSQL?
# 2. What is RSpec?
# 3. Why use `--skip-test` when generating Rails if we plan to use RSpec?
```

### Step 2: Database variables
```
# Replace config/database.yml with:

default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV["POSTGRES_USER"] || "postgres" %>
  password: <%= ENV["POSTGRES_PASSWORD"] || "postgres" %>
  host: <%= ENV["POSTGRES_HOST"] || "localhost" %>

development:
  <<: *default
  database: <%= ENV["POSTGRES_DATABASE"] || "deal_sourcing_api_development" %>

test:
  <<: *default
  database: <%= ENV["POSTGRES_TEST_DATABASE"] || "deal_sourcing_api_test" %>

production:
  <<: *default
  database: <%= ENV["POSTGRES_PRODUCTION_DATABASE"] %>


# Questions:
# 1: What is the purpose of `default` above?
# 2: Why do we set the env. variables?
```

### Step 3: Create the database
```
rails db:create

# Questions:
# 1. What databases are created with `rails db:create`?
# 2. Why do we need those databases?
```

### Step 4: Configure Environment-Specific Settings
```
# config/environments/development.rb
Rails.application.configure do
  # Other configurations...

  # Set the default URL options for the development environment
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
end

# config/environments/test.rb
Rails.application.configure do
  # Other configurations...

  # Set the default URL options for the test environment
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
end

# config/environments/production.rb
Rails.application.configure do
  # Other configurations...

  # Set the default URL options for the production environment
  config.action_mailer.default_url_options = { host: 'yourdomain.com', protocol: 'https' }
end


# Questions:
# 1. Where do you configure the default URL options for different environments?
# 2. What host and port are set for the development environment?
# 3. What protocol is set for the production environment?
```

### Step 5: Replace Gemfile
```
source "https://rubygems.org"

ruby "3.3.1"

# Core Gems
gem "rails", "~> 7.1.3", ">= 7.1.3.3"
gem "pg", "~> 1.1"  # Use PostgreSQL as the database for Active Record
gem "puma", ">= 5.0"  # Use the Puma web server

# Authentication
gem 'devise', '~> 4.8'
gem 'devise-jwt', '~> 0.11.0'

# GraphQL
gem 'graphql', '~> 2.3', '>= 2.3.2'
gem 'graphiql-rails', '~> 1.7', group: :development

# Email
gem 'postmark-rails', '~> 0.22.1'

# Utility
gem "tzinfo-data", platforms: %i[ windows jruby ]  # Windows time zone data
gem "bootsnap", require: false  # Reduces boot times through caching

# Asset Pipeline
gem 'propshaft', '~> 0.9.0', group: :development

# Development and Test Gems
group :development, :test do
  gem 'rspec-rails', '~> 6.1', '>= 6.1.2'
  gem 'factory_bot_rails', '~> 6.4', '>= 6.4.3'
  gem 'database_cleaner-active_record', '~> 2.1'
  gem 'faker', '~> 3.4', '>= 3.4.1'
  gem 'simplecov', '~> 0.22.0', require: false
  gem 'dotenv-rails', '~> 3.1', '>= 3.1.2'
  gem "debug", platforms: %i[ mri windows ]  # Debugging tools
end

group :development do
  # Speed up commands on slow machines / big apps [https://github.com/rails/spring]
  # gem "spring"
  gem 'rubocop-rails', '~> 2.25'
end
```

### Step 5: Start the Server
```
# Start the server to run your Rails application
rails server

# Questions:
# 1. What command starts the Rails server?
# 2. On which port does the Rails server run by default?
# 3. How do you stop the Rails server?
```
