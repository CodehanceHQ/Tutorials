### Using Modules as Namespaces

# - Modules can be used to group related classes, methods, and constants together.
# - This helps in organizing code and avoiding name clashes.
# - You will notice we use this alot within ruby on rails.

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