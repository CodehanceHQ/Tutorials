### Introduction
In this lesson, we will explore the fundamental building blocks of writing Ruby code, specifically focusing on classes, methods, properties, and objects. By the end of this tutorial, you will understand how each component works and be able to write and modify Ruby code with confidence.

### Create a Ruby Project
**Create a new directory for your project**:
```
cd ~/Desktop
mkdir UserProfileApp
cd UserProfileApp
```

**Create a file for the UserProfile class**:
```
touch user_profile.rb
touch main.rb
```

**Edit the user_profile.rb class**:
```
# user_profile.rb

# Define the UserProfile class
class UserProfile
  # Properties of the class
  attr_accessor :username, :email, :age, :is_logged_in

  # Constructor to initialize the properties
  def initialize(username, email, age)
    @username = username
    @email = email
    @age = age
    @is_logged_in = false # Default value
  end

  # Method to log in the user
  def login
    @is_logged_in = true
  end

  # Method to log out the user
  def logout
    @is_logged_in = false
  end

  # Method to update the email
  def update_email(new_email)
    @email = new_email
  end
end
```
**Edit the main.rb file**
```
# main.rb

# Require the user_profile.rb file to use the UserProfile class
require_relative 'user_profile'

# Create an object from the UserProfile class
user = UserProfile.new('johnDoe', 'john@example.com', 30)

# Accessing properties of the object
puts "Username: #{user.username}"   # Output: johnDoe
puts "Email: #{user.email}"         # Output: john@example.com
puts "Age: #{user.age}"             # Output: 30
puts "Logged In: #{user.is_logged_in}" # Output: false

# Calling methods on the object
user.login
puts "Logged In after login: #{user.is_logged_in}" # Output: true

user.update_email('john.doe@newmail.com')
puts "Updated Email: #{user.email}" # Output: john.doe@newmail.com

user.logout
puts "Logged In after logout: #{user.is_logged_in}" # Output: false
```

**Run the main.rb**:
```
ruby main.rb
```