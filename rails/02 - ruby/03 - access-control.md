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

### Private Methods
```
# - Private methods can only be called within the context of the current object.
# - They cannot be called with an explicit receiver, even from within the same class.

# 1. Create a file named `private_example.rb`.
# 2. Add the following code to `private_example.rb`:

class PrivateExample
  def public_method
    "I am a public method."
  end

  private

  def private_method
    "I am a private method."
  end
end

# Create an instance of PrivateExample and call the public method
example = PrivateExample.new
puts example.public_method  # Output: "I am a public method."

# Attempting to call the private method will raise an error
begin
  puts example.private_method
rescue => e
  puts e.message  # Output: "private method `private_method' called for #<PrivateExample:...>"
end
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

# Answer: The output will be "I am private." because private methods can be called from within other methods in the same class.

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

# Answer: The code will raise a NoMethodError because private methods cannot be called with an explicit receiver. 
# The correct code should call private_method without `self`:
class Test2
  def public_method
    private_method
  end
end

# Question 3: Explain why the following code does not work and how to fix it:
class Test3
  protected

  def protected_method
    "I am protected."
  end
end

test3 = Test3.new
puts test3.protected_method

# Answer: The code will raise a NoMethodError because protected methods cannot be called with an explicit receiver from outside the class. 
# The correct code should access protected_method within another method in the same class or subclass.

class Test3
  def call_protected
    protected_method
  end
end

test3 = Test3.new
puts test3.call_protected  # Output: "I am protected."

# Question 4: What will be the output of the following code, and why?
class Test4
  def initialize(value)
    @value = value
  end

  def compare_values(other)
    self.value == other.value
  end

  protected

  def value
    @value
  end
end

test4a = Test4.new(10)
test4b = Test4.new(10)
test4c = Test4.new(20)

puts test4a.compare_values(test4b)  # Output: true
puts test4a.compare_values(test4c)  # Output: false

# Question 5: Identify the mistake in the following code and provide the correct version:
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

# Answer: The code will raise a NoMethodError on the second call because private_method cannot be called with an explicit receiver.
# The correct code should call private_method without `self` in another_public_method:
class Test5
  def public_method
    private_method
  end

  def another_public_method
    private_method
  end
end

test5 = Test5.new
puts test5.public_method         # Output: "I am private."
puts test5.another_public_method # Output: "I am private."

# Question 6: Explain why the following code snippet does not produce the expected output and correct it:
class Test6
  def initialize(value)
    @value = value
  end

  def compare_values(other)
    value == other.value
  end

  protected

  def value
    @value
  end
end

test6a = Test6.new(10)
test6b = Test6.new(10)
test6c = Test6.new(20)

puts test6a.compare_values(test6b)  # Output: true
puts test6a.compare_values(test6c)  # Output: false
```