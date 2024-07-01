# Classes and Objects in Ruby

### Introduction to Classes
```
# - Classes are blueprints for creating objects.
# - They define properties (attributes) and behaviors (methods) for the objects.

## Defining a Class
# - A class is defined using the `class` keyword followed by the class name.

# 1. Create a file named `person.rb`.
# 2. Add the following code to `person.rb`:

class Person
  # The initialize method is called when a new object is created
  def initialize(name, age)
    @name = name  # Instance variable @name
    @age = age    # Instance variable @age
  end

  # Define an instance method
  def greet
    "Hello, my name is #{@name} and I am #{@age} years old."
  end
end

## Creating Objects
# - Objects are instances of a class.
# - You create an object by calling the `new` method on the class.

# 1. Create a file named `test_person.rb`.
# 2. Add the following code to `test_person.rb`:

require_relative 'person'

# Create an instance of Person and call the greet method
person = Person.new("Alice", 30)
puts person.greet  # Output: "Hello, my name is Alice and I am 30 years old."

## Instance Variables
# - Instance variables are variables that belong to an instance of a class.
# - They are prefixed with `@` and can be accessed within instance methods.

# 1. Create a file named `car.rb`.
# 2. Add the following code to `car.rb`:

class Car
  def initialize(make, model)
    @make = make   # Instance variable @make
    @model = model # Instance variable @model
  end

  def details
    "This car is a #{@make} #{@model}."
  end
end

# 1. Create a file named `test_car.rb`.
# 2. Add the following code to `test_car.rb`:

require_relative 'car'

# Create an instance of Car and call the details method
car = Car.new("Toyota", "Camry")
puts car.details  # Output: "This car is a Toyota Camry."
```


### Class Variables
```
# - Class variables are shared among all instances of a class.
# - They are prefixed with `@@` and can be accessed within class methods.

# 1. Create a file named `dog.rb`.
# 2. Add the following code to `dog.rb`:

class Dog
  @@species = "Canine"  # Class variable @@species

  def self.species
    @@species
  end

  def initialize(name)
    @name = name  # Instance variable @name
  end

  def details
    "This dog's name is #{@name} and it belongs to the #{@@species} species."
  end
end

# 1. Create a file named `test_dog.rb`.
# 2. Add the following code to `test_dog.rb`:

require_relative 'dog'

# Create an instance of Dog and call the details method
dog = Dog.new("Buddy")
puts dog.details       # Output: "This dog's name is Buddy and it belongs to the Canine species."
puts Dog.species       # Output: "Canine"

## Class Methods
# - Class methods are called on the class itself.
# - They are defined using `self` in the method definition.

# 1. Create a file named `math_helper.rb`.
# 2. Add the following code to `math_helper.rb`:

class MathHelper
  def self.square(number)
    number * number
  end
end

# 1. Create a file named `test_math_helper.rb`.
# 2. Add the following code to `test_math_helper.rb`:

require_relative 'math_helper'

# Call the class method square on MathHelper
puts MathHelper.square(5)  # Output: 25
```


### Tricky Questions to Find Bugs
```
# Question 1: Given the following code, what will be the output and why?
class Cat
  def initialize(name)
    @name = name
  end

  def self.meow
    "Meow!"
  end
end

cat = Cat.new("Whiskers")
puts cat.meow

# Question 2: Identify the bug in the following code:
class Bird
  def initialize(species)
    @species = species
  end

  def self.fly
    "I can fly!"
  end
end

bird = Bird.new("Parrot")
puts bird.fly

# Question 3: Explain why the following code does not work and how to fix it:
class Fish
  @@species = "Aquatic"

  def initialize(name)
    @name = name
  end

  def self.details
    "This fish belongs to the #{@@species} species."
  end
end

fish = Fish.new("Goldfish")
puts fish.details

# Question 4: What will be the output of the following code, and why?
class Train
  def initialize(type)
    @type = type
  end

  def self.travel
    "Choo Choo!"
  end
end

train = Train.new("Freight")
puts train.travel

# Question 5: Identify the mistake in the following code and provide the correct version:
class House
  def initialize(address)
    @address = address
  end

  def self.location
    "This house is located at #{@address}."
  end
end

house = House.new("123 Main St")
puts house.location

# Question 6: Explain why the following code snippet does not produce the expected output and correct it:
class Train
  def initialize(type)
    @type = type
  end

  def self.travel
    "This train is a #{@type} train and it goes Choo Choo!"
  end
end

train = Train.new("Freight")
puts train.travel
```