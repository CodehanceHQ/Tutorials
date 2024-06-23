## Creating & Importing Classes


### Introduction
Welcome to the course! In this lesson, we will explore the fundamental building blocks of writing Ionic Angular code. By the end of this tutorial, you will understand how each component works and be able to write and modify Angular code with confidence.

### Stop any previously running ionic app
```
First, ensure that any Ionic app you may have running is stopped.
```

### Create blank project
In your terminal or iTerm do the following:
```
cd ~/Desktop
ionic start UserProfileApp blank --type=angular

# select NgModules
```

### Install Dependencies
```
npm install
```

### Create model directory and user-profile.ts
Still in your terminal or iTerm do the following:
```
cd UserProfileApp
mkdir -p src/app/models
touch src/app/models/user-profile.ts
```

### Open project in VSCode
```
code .
```

### Folder directory
Here is the directory structure of your project. We added models folder ourselves and we also added user-profile.ts within it so we can learn the fundamentals.
```
UserProfileApp/
├── src/
│   ├── app/
│   │   ├── models/
│   │   │   └── user-profile.ts
│   │   ├── home/
│   │   │   ├── home.page.html
│   │   │   ├── home.page.scss
│   │   │   ├── home.page.ts
│   │   ├── app.component.html
│   │   ├── app.component.scss
│   │   ├── app.component.ts
│   │   ├── app.module.ts
│   │   ├── app-routing.module.ts
├── ...
```

### update the user-profile.ts
```typescript
// src/app/models/user-profile.ts

// Define the UserProfile class
export class UserProfile {
  // Define properties for the class
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

### Import into Home Component
Take note of the difference between what Home Component looked like before you entered this code.
```
// src/app/home/home.page.ts

import { Component } from '@angular/core';
import { UserProfile } from '../models/user-profile';

@Component({
  selector: 'app-home',
  templateUrl: 'home.page.html',
  styleUrls: ['home.page.scss'],
})
export class HomePage {
  // Create an instance of UserProfile
  user: UserProfile;
  newEmail: string = ''; // Property to hold new email input

  // Initialize the user instance with default values
  constructor() {
    this.user = new UserProfile('johnDoe', 'john@example.com', 30);
  }

  // Method to log in the user
  loginUser() {
    this.user.login();
    console.log(this.user.isLoggedIn); // true
  }

  // Method to log out the user
  logoutUser() {
    this.user.logout();
    console.log(this.user.isLoggedIn); // false
  }

  // Method to update the user's email
  updateEmail(newEmail: string) {
    this.user.updateEmail(newEmail);
    console.log(this.user.email); // new email
  }
}

```

### Replace Home Page HTML
```
<!-- src/app/home/home.page.html -->

<ion-header>
  <ion-toolbar>
    <ion-title>
      User Profile
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content>
  <!-- Display user information -->
  <p>Username: {{ user.username }}</p>
  <p>Email: {{ user.email }}</p>
  <p>Age: {{ user.age }}</p>
  <p>Logged In: {{ user.isLoggedIn }}</p>
  
  <!-- Buttons to log in and log out -->
  <ion-button (click)="loginUser()">Login</ion-button>
  <ion-button (click)="logoutUser()">Logout</ion-button>
  
  <!-- Input to update email -->
  <ion-input placeholder="New Email" [(ngModel)]="newEmail"></ion-input>
  <ion-button (click)="updateEmail(newEmail)">Update Email</ion-button>
</ion-content>
```

### Run server
```
ionic serve
```