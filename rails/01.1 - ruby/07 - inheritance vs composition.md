# Inheritance vs. Composition in Ruby

### Introduction to Inheritance
```
# - Inheritance allows a class to inherit behavior from another class.
# - This promotes code reuse but can lead to tight coupling and less flexible code.

# Define a parent class Animal
class Animal
  def speak
    "I can speak!"
  end
end

# Define a child class Dog that inherits from Animal
class Dog < Animal
  def bark
    "Woof!"
  end
end

# Create an instance of Dog and call the inherited speak method and the bark method
dog = Dog.new
puts dog.speak  # Output: "I can speak!"
puts dog.bark   # Output: "Woof!"
```

### Introduction to Composition
```
# - Composition allows a class to reuse behavior from multiple classes or modules.
# - This promotes more flexible and modular code by favoring composition over inheritance.

# Define a module Walkable with a walk method
module Walkable
  def walk
    "I can walk!"
  end
end

# Define a module Runnable with a run method
module Runnable
  def run
    "I can run!"
  end
end

# Define a class Person that includes Walkable and Runnable modules
class Person
  include Walkable
  include Runnable
end

# Create an instance of Person and call the walk and run methods
person = Person.new
puts person.walk  # Output: "I can walk!"
puts person.run   # Output: "I can run!"
```

### Comparing Inheritance and Composition
```
# Example using Inheritance
class Vehicle
  def start_engine
    "Engine started!"
  end
end

class Car < Vehicle
  def drive
    "Driving a car!"
  end
end

car = Car.new
puts car.start_engine  # Output: "Engine started!"
puts car.drive         # Output: "Driving a car!"

# Example using Composition
module Startable
  def start_engine
    "Engine started!"
  end
end

class Motorcycle
  include Startable
  
  def ride
    "Riding a motorcycle!"
  end
end

motorcycle = Motorcycle.new
puts motorcycle.start_engine  # Output: "Engine started!"
puts motorcycle.ride          # Output: "Riding a motorcycle!"
```

### Tricky Questions to Find Bugs
```
# Question 1: Given the following code, what will be the output and why?
class Mammal
  def speak
    "I am a mammal!"
  end
end

class Cat < Mammal
  def purr
    "Purr!"
  end
end

puts Cat.speak

# Question 2: Identify the bug in the following code:
class Engine
  def start
    "Engine started!"
  end
end

class Truck < Engine
end

truck = Truck.new
puts truck.drive

# Question 3: Explain why the following code does not work and how to fix it:
module Flyable
  def fly
    "I can fly!"
  end
end

class Bird
  include Flyable
end

puts Bird.fly

# Question 4: What will be the output of the following code, and why?
module Drivable
  def drive
    "I can drive!"
  end
end

class Car
  include Drivable
end

puts Car.drive

# Question 5: Identify the mistake in the following code and provide the correct version:
module Runnable
  def run
    "I can run!"
  end
end

class Athlete
  include Runnable
end

puts Runnable.run

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