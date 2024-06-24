# Types of Methods in Ruby: Instance Methods vs. Class Methods

### Introduction to Methods
```
# - Methods are used to perform actions and are defined within classes or modules.
# - Ruby has two main types of methods: instance methods and class methods.
```

### Instance Methods
```
# - Instance methods are called on instances of a class.
# - They operate on specific objects and can access instance variables.

# Define a class Animal with an instance method speak
class Animal
  def speak
    "I can speak!"
  end
end

# Create an instance of Animal and call the speak method
animal = Animal.new
puts animal.speak  # Output: "I can speak!"
```

### Class Methods
```
# - Class methods are called on the class itself.
# - They operate on the class as a whole and cannot access instance variables directly.

# Define a class Vehicle with a class method type
class Vehicle
  def self.type
    "I am a vehicle!"
  end
end

# Call the type method on the Vehicle class
puts Vehicle.type  # Output: "I am a vehicle!"
```

### Comparing Instance Methods and Class Methods
```
# Example with Instance Methods
class Person
  def initialize(name)
    @name = name
  end

  def greet
    "Hello, my name is #{@name}."
  end
end

person = Person.new("Alice")
puts person.greet  # Output: "Hello, my name is Alice."

# Example with Class Methods
class Calculator
  def self.add(a, b)
    a + b
  end
end

puts Calculator.add(5, 3)  # Output: 8
```

### Combining Instance and Class Methods
```
# - A class can have both instance and class methods.
# - Use instance methods for actions related to specific objects and class methods for actions related to the class.

class Book
  def self.category
    "Literature"
  end

  def initialize(title)
    @title = title
  end

  def details
    "Title: #{@title}, Category: #{self.class.category}"
  end
end

book = Book.new("1984")
puts book.details  # Output: "Title: 1984, Category: Literature"
```

### Tricky Questions to Find Bugs
```
# Question 1: Given the following code, what will be the output and why?
class Cat
  def self.sound
    "Meow"
  end
end

cat = Cat.new
puts cat.sound

# Question 2: Identify the bug in the following code:
class Dog
  def self.bark
    "Woof!"
  end
end

dog = Dog.new
puts dog.bark

# Question 3: Explain why the following code does not work and how to fix it:
class Bird
  def sing
    "Tweet!"
  end
end

puts Bird.sing

# Question 4: What will be the output of the following code, and why?
class Fish
  def self.swim
    "I can swim!"
  end
end

fish = Fish.new
puts fish.swim

# Question 5: Identify the mistake in the following code and provide the correct version:
class Car
  def drive
    "Vroom!"
  end
end

puts Car.drive

# Question 6: Explain why the following code snippet does not produce the expected output and correct it:
class Train
  def self.travel
    "Choo Choo!"
  end
end

train = Train.new
puts train.travel
```