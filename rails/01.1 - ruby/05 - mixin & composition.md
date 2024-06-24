# Mixins and Composition in Ruby
```
## Introduction to Mixins
# - Mixins allow classes to share reusable code across multiple classes.
# - Modules are used to create mixins in Ruby.
```

### Defining a Mixin
```
# 1. Create a file named `mixin_example.rb`.
# 2. Add the following code to `mixin_example.rb`:

# Define the Flyable module with a fly method
module Flyable
  def fly
    "I can fly!"
  end
end

# Define the Bird class that includes the Flyable module
class Bird
  include Flyable
end

# Define the Airplane class that includes the Flyable module
class Airplane
  include Flyable
end

# Create instances of Bird and Airplane
bird = Bird.new
airplane = Airplane.new

# Call the fly method on both instances and print the result
puts bird.fly  # Output: "I can fly!"
puts airplane.fly  # Output: "I can fly!"
```

### Using Mixins for Composition
```
# - Mixins can be used to add behavior to classes without using inheritance.
# - This promotes code reuse and composition over inheritance.

# Define a module Walkable with a walk method
module Walkable
  def walk
    "I can walk!"
  end
end

# Define the Human class that includes both Walkable and Flyable modules
class Human
  include Walkable
  include Flyable
end

# Create an instance of Human
human = Human.new

# Call the walk and fly methods on the Human instance and print the result
puts human.walk  # Output: "I can walk!"
puts human.fly   # Output: "I can fly!"
```

### Tricky Questions to Find Bugs
```
# Question 1: Given the following code, what will be the output and why?
module Swimable
  def swim
    "I can swim!"
  end
end

class Fish
  include Swimable
end

puts Fish.swim

# Question 2: Identify the bug in the following code:
module Runnable
  def run
    "I can run!"
  end
end

class Cheetah
  include Runnable
end

cheetah = Cheetah.new
puts Cheetah.run

# Question 3: Explain why the following code does not work and how to fix it:
module Drivable
  def drive
    "I can drive!"
  end
end

class Car
  include Drivable
end

puts Drivable.drive

# Question 4: What will be the output of the following code, and why?
module Eatable
  def eat
    "I can eat!"
  end
end

class Person
  include Eatable
end

puts Person.eat

# Question 5: Identify the mistake in the following code and provide the correct version:
module Singable
  def sing
    "I can sing!"
  end
end

class Singer
  include Singable
end

puts Singable.sing

# Question 6: Explain why the following code snippet does not produce the expected output and correct it:
module Paintable
  def paint
    "I can paint!"
  end
end

class Artist
  include Paintable
end

artist = Paintable.new
puts artist.paint
```