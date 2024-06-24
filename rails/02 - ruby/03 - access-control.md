# Access Control in Ruby: Public, Private, and Protected Methods

### Introduction to Access Control
```
# - Ruby provides three levels of access control: public, private, and protected.
# - These control the visibility of methods and variables in classes.
```

### Public Methods
```
# - Public methods can be called by anyone - from inside or outside the class.
# - By default, all methods in Ruby are public unless specified otherwise.

# 1. Create a file named `public_example.rb`.
# 2. Add the following code to `public_example.rb`:

class PublicExample
  def public_method
    "I am a public method."
  end
end

# Create an instance of PublicExample and call the public method
example = PublicExample.new
puts example.public_method  # Output: "I am a public method."
```

### Protected Methods
```
# - Protected methods can be called by any instance of the defining class or its subclasses.
# - They are accessible within the same family of objects.

# 1. Create a file named `protected_example.rb`.
# 2. Add the following code to `protected_example.rb`:

class Person
  def initialize(name, age)
    @name = name
    @age = age
  end

  def older_than?(other)
    if other.is_a?(Person)
      self.age > other.age
    else
      false
    end
  end

  protected

  def age
    @age
  end
end

# Create instances of Person
person1 = Person.new("Alice", 30)
person2 = Person.new("Bob", 25)

# Compare ages using the protected method
puts person1.older_than?(person2)  # Output: true
puts person2.older_than?(person1)  # Output: false

# Attempt to access the protected method directly will raise an error
begin
  puts person1.age
rescue => e
  puts e.message  # Output: "protected method `age' called for #<Person:...>"
end

# Attempt to access the protected method from outside the class will raise an error
class Stranger
  def reveal_age(person)
    person.age
  end
end

stranger = Stranger.new
begin
  puts stranger.reveal_age(person1)
rescue => e
  puts e.message  # Output: "protected method `age' called for #<Person:...>"
end
```

### Private Methods
```
### Private Methods
# - Private methods can only be called within the same instance of the class.
# - You cannot call a private method with the object name or `self.`.
# - Private methods can only be called directly by their name within the class.
# - Private methods cannot be called from a subclass.
# - Private methods cannot be called from outside the class, even through composition.

# 1. Create a file named `private_example.rb`.
# 2. Add the following code to `private_example.rb`:

class Parent
  def public_method
    "I am a public method."
  end

  def call_private_method
    private_method
  end

  private

  def private_method
    "I am a private method."
  end
end

class Child < Parent
  def attempt_private_call
    private_method
  end
end

class Composer
  def initialize(parent)
    @parent = parent
  end

  def call_composed_private_method
    @parent.private_method
  end
end

# Create an instance of Parent and call the public method
parent = Parent.new
puts parent.public_method  # Output: "I am a public method."

# Call the private method indirectly through another public method
puts parent.call_private_method  # Output: "I am a private method."

# Attempting to call the private method directly will raise an error
begin
  puts parent.private_method
rescue => e
  puts e.message  # Output: "private method `private_method' called for #<Parent:...>"
end

# Attempting to call the private method from a subclass will raise an error
child = Child.new
begin
  puts child.attempt_private_call
rescue => e
  puts e.message  # Output: "private method `private_method' called for #<Child:...>"
end

# Attempting to call the private method through composition will raise an error
composer = Composer.new(parent)
begin
  puts composer.call_composed_private_method
rescue => e
  puts e.message  # Output: "private method `private_method' called for #<Parent:...>"
end
```


### Tricky Questions to Find Bugs
```
# Question 1: Given the following code, what will be the output and why?
class Test1
  def public_method
    private_method
  end

  private

  def private_method
    "I am private."
  end
end

test1 = Test1.new
puts test1.public_method

# Question 2: Identify the bug in the following code:
class Test2
  def public_method
    self.private_method
  end

  private

  def private_method
    "I am private."
  end
end

test2 = Test2.new
puts test2.public_method

# Question 3: Explain why the following code does not work and how to fix it:
class Test3
  protected

  def protected_method
    "I am protected."
  end
end

test3 = Test3.new
puts test3.protected_method

class Test3
  def call_protected
    protected_method
  end
end

test3 = Test3.new
puts test3.call_protected  # Output: "I am protected."

# Question 4: Identify the mistake in the following code and provide the correct version:
class Test5
  def public_method
    private_method
  end

  def another_public_method
    self.private_method
  end

  private

  def private_method
    "I am private."
  end
end

test5 = Test5.new
puts test5.public_method
puts test5.another_public_method

# Question 5: What will be the output of the following code, and why?

class Parent
  def initialize(name, age)
    @name = name
    @age = age
  end

  def compare_ages(other)
    age_difference(other)
  end

  protected

  def age
    @age
  end

  def age_difference(other)
    self.age - other.age
  end
end

class Composer
  def initialize(parent)
    @parent = parent
  end

  def access_protected_method(other)
    @parent.age_difference(other)
  end
end

# Create instances of Parent
parent1 = Parent.new("Alice", 30)
parent2 = Parent.new("Bob", 25)
composer = Composer.new(parent1)

# Compare ages directly within Parent class
puts parent1.compare_ages(parent2)  # Output: 5

# Attempt to access the protected method through composition
begin
  puts composer.access_protected_method(parent2)
rescue => e
  puts e.message  # Output: protected method `age_difference' called for #<Parent:...>
end

```