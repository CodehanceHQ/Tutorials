# Introduction to Ruby Modules

## Definition and Purpose
- Modules are a way to encapsulate methods, constants, and other modules in Ruby.
- They help avoid name collisions and provide a mechanism for namespacing.

## Including and Extending Modules
- `include` adds instance methods from a module to a class.
- `extend` adds class methods from a module to a class.

# Defining Modules

## Basic Module Syntax
module MyModule
  CONSTANT = "I am a constant"
  
  def self.module_method
    "I am a module method"
  end
  
  def instance_method
    "I am an instance method"
  end
end


## Using Modules as Namespaces
- Modules can be used to group related classes, methods, and constants together.
- This helps in organizing code and avoiding name clashes.

# Nesting Modules and Classes

## Defining Nested Modules and Classes

module OuterModule
  CONSTANT = "Outer constant"
  
  module InnerModule
    CONSTANT = "Inner constant"
    
    class MyClass
      def self.class_method
        "I am a class method within InnerModule::MyClass"
      end
    end
  end
end


## Accessing Nested Constants and Methods
- Use the scope resolution operator (`::`) to access nested constants and methods.

puts OuterModule::CONSTANT
puts OuterModule::InnerModule::CONSTANT
puts OuterModule::InnerModule::MyClass.class_method


module Flyable
  def fly
    "I can fly!"
  end
end

class Bird
  include Flyable
end

class Airplane
  include Flyable
end

puts Bird.new.fly
puts Airplane.new.fly


## Best Practices
- Use modules and nested structures to enhance maintainability and readability.
- Avoid excessive nesting to prevent complexity.

# Advanced Concepts

## Mixins and Composition
- Modules can be used to share behavior between classes, known as mixins.
- Composition using modules is often preferred over inheritance.

## Module Callbacks (Hooks)
- `included`, `extended`, and `prepended` are hooks that can be used to modify behavior when modules are included or extended.


module MyModule
  def self.included(base)
    puts "#{base} has included #{self}"
  end
end

class MyClass
  include MyModule
end


## Refinements
- Refinements allow you to modify classes within specific scopes.
- Useful for temporary or scoped changes to classes.


module StringExtensions
  refine String do
    def shout
      upcase + "!!!"
    end
  end
end

using StringExtensions

puts "hello".shout


# Summary and Q&A

## Review Key Concepts
- Recap the importance of modules and nesting in Ruby.
- Highlight how they help in organizing and modularizing code.

## Questions and Answers
- Provide an opportunity for students to ask questions and clarify doubts.
- Discuss how these concepts can be applied in real projects.
