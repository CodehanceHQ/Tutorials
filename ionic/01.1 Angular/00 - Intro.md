## Understanding Classes in Ionic Angular: A Beginner's Guide

### Introduction
Welcome to the course! In this lesson, we will explore the fundamental concepts of classes, methods, properties, and objects in Ionic Angular. By the end of this tutorial, you will understand how each component works and how they are used in Angular development.

### Stop any previously running ionic app
First, ensure that any Ionic app you may have running is stopped.

### Classes
Classes are blueprints for creating objects. They encapsulate data for the object and methods to manipulate that data. In Ionic Angular, classes help in structuring your code by defining reusable components.

#### Example
Consider a `UserProfile` class. This class defines the structure and behavior of user profiles in your application. It might include properties like `username`, `email`, and `age`, and methods like `login`, `logout`, and `updateEmail`. When you create an object from this class, such as a specific user's profile, the object will have its own `username`, `email`, and `age`, and can use the defined methods to perform actions like logging in or updating the email address.

```
export class UserProfile {
  // Properties of the class
  username: string;
  email: string;
  age: number;
  isLoggedIn: boolean;

  // Constructor to initialize the properties
  constructor(username: string, email: string, age: number) {
    this.username = username;
    this.email = email;
    this.age = age;
    this.isLoggedIn = false; // Default value
  }

  // Method to log in the user
  login() {
    this.isLoggedIn = true;
  }

  // Method to log out the user
  logout() {
    this.isLoggedIn = false;
  }

  // Method to update the email
  updateEmail(newEmail: string) {
    this.email = newEmail;
  }
}
```

### Methods
Methods are functions defined within a class that describe the behaviors of an object. They allow objects to perform actions. 

#### Example
In the `UserProfile` exmaple, methods would be things like `login`, `logout`, and `updateEmail`.

```
// Method to log in the user
login() {
  this.isLoggedIn = true;
}

// Method to log out the user
logout() {
  this.isLoggedIn = false;
}

// Method to update the email
updateEmail(newEmail: string) {
  this.email = newEmail;
}
```

### Properties
Properties are variables that hold data associated with a class and its objects. They define the characteristics of an object.

#### Example
Example property would be those things that make up a UserProfile, its fields such as:  `username`, `email`, and `age`.

```
// Properties of the class
username: string;
email: string;
age: number;
isLoggedIn: boolean;
```

### Objects
Objects are instances of a class. They represent real-world entities and are created using the class blueprint. An object has its own unique values for the properties defined by the class.

```
// Create a `john` object from the UserProfile class
const john = new UserProfile('johnDoe', 'john@example.com', 30);

// Accessing properties of the object
console.log(john.username); // Output: johnDoe
console.log(john.email); // Output: john@example.com
console.log(john.age); // Output: 30
console.log(john.isLoggedIn); // Output: false
```

### Practical Example (Theoretical Explanation)
Let's consider a theoretical example to illustrate these concepts:

#### UserProfile Class
Imagine we have a `UserProfile` class that represents a user profile in an application.

- **Properties**:
  - `username`: The username of the user.
  - `email`: The email address of the user.
  - `age`: The age of the user.
  - `isLoggedIn`: A boolean indicating if the user is logged in.

- **Methods**:
  - `login()`: A method to log the user in.
  - `logout()`: A method to log the user out.
  - `updateEmail(newEmail)`: A method to update the user's email.

#### Creating an Object
When we create a new user profile, we are creating an instance of the `UserProfile` class. For example, a user profile with the username "johnDoe" and email "john@example.com" is an object created from the `UserProfile` class.

### Summary
In this lesson, we've covered the basic concepts of classes, methods, properties, and objects in Ionic Angular. These are fundamental building blocks that will help you understand how to structure and organize your code effectively.