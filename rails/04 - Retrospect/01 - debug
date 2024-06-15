# Ruby Debugging with the `debug`

# Step 1: Install the `debug` Gem
```
# Add the `debug` gem to your Gemfile if not there
group :development, :test do
  gem "debug", platforms: %i[mri windows]  # Debugging tool
end

# Run `bundle install` to install the gem.
```

# Step 2: Setting Breakpoints
```
# Insert `binding.break` as shown into your graphql_controller as below

def execute
  binding.break # <<<<< added break here

  variables = prepare_variables(params[:variables])
  query = params[:query]
  operation_name = params[:operationName]
  token = request.headers['Authorization']&.split(' ')&.last

  context = {
    current_user: set_current_user,
    jwt_token: token,
    jwt_payload: decode_jwt_payload(token)
  }
  binding.break # <<<<< added break here

  result = DealSourcingApiSchema.execute(query, variables: variables, context: context, operation_name: operation_name)
  render json: result
rescue StandardError => e
  raise e unless Rails.env.development?
  handle_error_in_development(e)
end
```

# Step 3: Running the Code
```
# Make a request that will call that controller either from graphql browser or through rspec

rspec

or 

http://127.0.0.1:3000/graphiql
```

# Step 4: Using Debug Commands
```
# When execution hits the `binding.break` breakpoint, you'll enter an interactive debugging session in the terminal.

# Basic Navigation Commands
# - `next` (or `n`): Move to the next line within the same context.
# - `step` (or `s`): Step into the next method call.
# - `continue` (or `c`): Continue execution until the next breakpoint or end of the program.
# - `finish` (or `fin`): Continue execution until the current stack frame returns.

# Modifying Variables
# - `variable_name = value`: Set a variable to a new value.

# Calling Methods
# - Call any method in the current scope to check its output:
# some_method

# Evaluating Expressions
# - Evaluate Ruby expressions directly in the Debug console:
# 1 + 1
```

# Step 5: Exiting Debug
```
# - `exit` or `quit`: Exit the Debug session.
```

# Conclusion
```
# This tutorial provides a basic introduction to using the `debug` gem for debugging Ruby and Rails applications. 
# It covers installation, setting breakpoints, using commands, and practical examples to get you started with effective debugging.
```
