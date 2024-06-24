### Using Modules as Namespaces
```
# - Modules can be used to group related classes, methods, and constants together.
# - This helps in organizing code and avoiding name clashes.
# - You will notice we use this alot within ruby on rails.
```

### Nesting Modules and Classes
```
## Defining Nested Modules and Classes

# 1. Create a directory structure as follows:
#    - app/
#      - models/
#        - outer_module.rb
#        - inner_module/
#          - my_class.rb
```
### 2. Add the following code to `outer_module.rb`:
```
module OuterModule
  module InnerModule
  end
end
```

### 3. Add the following code to `inner_module/my_class.rb`:
```
module OuterModule
  module InnerModule
    class MyClass
      def self.class_method
        "I am a class method within InnerModule::MyClass"
      end
    end
  end
end

# Accessing Nested Methods
# Use the scope resolution operator (::) to access nested methods.
```
### 4. Create a file `test_modules.rb` in the root directory with the following code:
```
require_relative 'app/models/outer_module'
require_relative 'app/models/inner_module/my_class'

puts OuterModule::InnerModule::MyClass.class_method

# Running the Test File
# 1. Open your terminal or command prompt.
# 2. Navigate to the root directory of your project.
# 3. Run the test file using the following command:
#    ruby test_modules.rb

# This will output:
# I am a class method within InnerModule::MyClass
```

### 5. Questions:
```
# 1: Given the following code, what will the output be and why?

module OuterModule
  module InnerModule
    class MyClass
      def self.greet
        "Hello from InnerModule::MyClass"
      end
    end
  end
end

puts OuterModule::InnerModule::MyClass.greet

# 2: Identify the bug in the following code:

module OuterModule
  module InnerModule
    class MyClass
      def greet
        "Hello from InnerModule::MyClass"
      end
    end
  end
end

puts OuterModule::InnerModule::MyClass.greet

# 3: Explain why the following code does not work and how to fix it:

module OuterModule
  class MyClass
    def self.greet
      "Hello from OuterModule::MyClass"
    end
  end
end

module OuterModule
  module InnerModule
    class MyClass
      def self.greet
        "Hello from InnerModule::MyClass"
      end
    end
  end
end

puts OuterModule::MyClass.greet

# 4: What will be the output of the following code, and why?

module OuterModule
  module InnerModule
    class MyClass
      def greet
        "Hello"
      end
    end
  end
end

module OuterModule
  class AnotherClass
    include InnerModule
  end
end

instance = OuterModule::AnotherClass.new
puts instance.greet


# 5: Identify the mistake in the following code and provide the correct version:

module OuterModule
  module InnerModule
    class MyClass
      def greet
        "Hello from InnerModule::MyClass"
      end
    end
  end
end

include OuterModule::InnerModule::MyClass

puts greet

# Explain why the following code snippet does not produce the expected output and correct it:

module OuterModule
  module InnerModule
    class MyClass
      def greet
        "Hello from MyClass"
      end
    end
  end
end

instance = OuterModule::MyClass.new
puts instance.greet
```