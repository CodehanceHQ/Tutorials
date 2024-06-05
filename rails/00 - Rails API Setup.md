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

### Create the database
```
rails db:create

# Questions:
# 1. What databases are created with `rails db:create`?
# 2. Why do we need those databases?
```

### Step 2: Configure Environment-Specific Settings
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

### Step 3: Start the Server
```
# Start the server to run your Rails application
rails server

# Questions:
# 1. What command starts the Rails server?
# 2. On which port does the Rails server run by default?
# 3. How do you stop the Rails server?
```
