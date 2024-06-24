### Tutorial: Understanding Access Control in Ruby: Private, Public, and Protected Methods

### Introduction
In Ruby, access control allows you to restrict how methods can be accessed within a class. The three levels of access control are `public`, `protected`, and `private`. Understanding these access levels helps in designing robust and secure object-oriented programs.

### Access Control Levels
1. **Public Methods**: Accessible from anywhere, both inside and outside the class.
2. **Protected Methods**: Accessible from within the class and its subclasses, but not from outside the class.
3. **Private Methods**: Accessible only within the class in which they are defined. They cannot be called with an explicit receiver, even from within the class.

### Create a Ruby Project
1. **Create a new directory for your project**:
```
mkdir AccessControlApp
cd AccessControlApp

touch user_profile.rb
touch main.rb
```

### user profile.rb
```
# user_profile.rb

# Define the UserProfile class with different access levels
class UserProfile
  attr_accessor :username, :email, :age

  # Constructor to initialize the instance variables
  def initialize(username, email, age)
    @username = username
    @age = age
    @is_logged_in = false # Default value
    set_email(email) # Using a private method
  end

  # Public method
  public
  def login
    @is_logged_in = true
    puts "User logged in"
  end

  def logout
    @is_logged_in = false
    puts "User logged out"
  end

  # Public method calling a protected method
  def display_status
    show_status
  end

  # Public method using a private method
  def change_email(new_email)
    set_email(new_email)
  end

  # Protected method
  protected
  def show_status
    puts "Username: #{@username}, Logged In: #{@is_logged_in}"
  end

  # Private method
  private
  def set_email(new_email)
    if new_email.include?('@')
      @email = new_email
    else
      puts "Invalid email format"
    end
  end
end
```

### main.rb
```
# main.rb

# Require the user_profile.rb file to use the UserProfile class
require_relative 'user_profile'

# Create an object from the UserProfile class
user = UserProfile.new('johnDoe', 'john@example.com', 30)

# Accessing public methods
user.login
user.logout

# Trying to access a protected method (will raise an error)
begin
  user.show_status
rescue NoMethodError => e
  puts e.message # Output: protected method `show_status' called for #<UserProfile:...>
end

# Accessing a public method that calls a protected method
user.display_status # Output: Username: johnDoe, Logged In: false

# Trying to access a private method (will raise an error)
begin
  user.set_email('new.email@example.com')
rescue NoMethodError => e
  puts e.message # Output: private method `set_email' called for #<UserProfile:...>
end

# Accessing a public method that uses a private method
user.change_email('new.email@example.com')
puts "Email: #{user.email}" # Output: Email: new.email@example.com

# Trying to set an invalid email
user.change_email('invalidemail.com')
puts "Email after invalid update attempt: #{user.email}" # Output: Invalid email format, Email: new.email@example.com
```

### run
```
ruby main.rb
```


### Test Questions

1. **What is the difference between public, protected, and private methods in Ruby?**
   - Describe how each access level controls the accessibility of methods within a class and outside of it.

2. **Why would you use a private method in a class? Provide an example scenario.**
   - Explain the purpose of private methods and give a situation where using a private method is beneficial.

3. **How can a public method in a class access a private method?**
   - Describe the mechanism by which a public method can call a private method within the same class, and why this might be useful.

4. **What happens if you try to call a protected method from outside the class it is defined in?**
   - Explain the expected behavior and the error message that would be raised.

5. **Provide an example where a protected method is useful in a subclass.**
   - Describe a scenario where a protected method in a superclass can be beneficial for use in its subclass, and give a brief code example.
