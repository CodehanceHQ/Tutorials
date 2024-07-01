### Basic Module Syntax
```
# Creating a Ruby File
# 1. Create a new file named `my_module.rb`.
# 2. Add the following code to `my_module.rb`:

module MyModule
  CONSTANT = "I am a constant"
  
  def self.module_method
    "I am a module method"
  end
  
  def instance_method
    "I am an instance method"
  end
end
```

### Running the Ruby File
```
# 1. Open your terminal or command prompt.
# 2. Navigate to the directory where you saved `my_module.rb`.
# 3. Run the file using the following command:
#    ruby my_module.rb
# 4. To test the module, you can create another Ruby file (e.g., `test_my_module.rb`) and add the following code:

require_relative 'my_module'

puts MyModule::CONSTANT
puts MyModule.module_method

class MyClass
  include MyModule
  
  def test_instance_method
    puts instance_method
  end
end

my_class_instance = MyClass.new
my_class_instance.test_instance_method
```

### Run the test file using the following command:
```
ruby test_my_module.rb

# This will output:
# I am a constant
# I am a module method
# I am an instance method
```

### Questions
```
# 1: What is a constant in Ruby?
# 2: What is the difference between module method vs instance method?
# 3: How did we access CONSTANT?
# 4: How does `require_relative` work?
# 5: How do we define instance method?
```