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
In this step, we are defining the `UserProfile` class in the `user_profile.rb` file. This class includes properties such as `username`, `email`, `age`, and `is_logged_in` which are initialized through a constructor method. 

The class also contains methods to manipulate these properties: `login` sets `is_logged_in` to true, `logout` sets it to false, and `update_email` changes the user's email to a new value. 

By defining this class, we create a blueprint for user profile objects that encapsulates user data and behaviors.
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

**Edit the main.rb file**:
In this step, we are editing the `main.rb` file to test the functionality of the `UserProfile` class. We start by requiring the `user_profile.rb` file to access the `UserProfile` class. We then create an instance of `UserProfile` with initial values for `username`, `email`, and `age`. 

We demonstrate accessing the object's properties and printing them to the console. Next, we call the `login` method to change the `is_logged_in` property to true and print the updated value. We update the user's email using the `update_email` method and print the new email. 

Finally, we call the `logout` method to set `is_logged_in` to false and print the updated state. This script helps us verify that the `UserProfile` class and its methods work as expected.

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

### Instances
An instance is a specific realization of any object created using a class. When you define a class, you are creating a blueprint for objects, but no actual object exists until you create an instance of that class. Each instance has its own unique set of properties and can use the methods defined in the class. 

For example, if you have a `UserProfile` class, you can create multiple instances of this class, each representing a different user profile. Each instance will have its own `username`, `email`, `age`, and `is_logged_in` property values. Creating an instance is done by calling the class with the `new` keyword and passing the necessary arguments to the constructor.

#### Example
In the `main.rb` file, we create an instance of the `UserProfile` class:

```
# john and jane here are the instances

john = UserProfile.new('johnDoe', 'john@example.com', 30)
jane = UserProfile.new('janeDoe', 'jane@example.com', 34)
```

**Run the main.rb**:
In you terminal root of this project run:
```
ruby main.rb
```

### Test Questions

1. **What is the purpose of the `initialize` method in the `UserProfile` class?**
   - Explain what the `initialize` method does and how it is used when creating an instance of the class.

2. **How do you create an instance of the `UserProfile` class? Provide an example.**
   - Describe the steps to create an instance and give an example using the `UserProfile` class.

3. **What are the properties defined in the `UserProfile` class, and how are they initialized?**
   - List the properties in the `UserProfile` class and explain how they are set up in the constructor.

4. **How do you call a method on an instance of the `UserProfile` class? Provide an example using the `login` method.**
   - Explain the process of calling a method on an instance and give an example with the `login` method.
