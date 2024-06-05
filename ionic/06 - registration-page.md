# follow these steps to generate registration page

```
// Generate a new page for the registration
ionic generate page registration

// src/app/registration/registration.module.ts

// This module sets up the registration page. It imports necessary modules and declares the RegistrationPage component.
// Questions:
// 1. What is the purpose of importing `ReactiveFormsModule` in the `RegistrationPageModule`?
// 2. What is the difference between ionic generate page and generate component?
// 3. What else can ionic generate?

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule, ReactiveFormsModule } from '@angular/forms';
import { IonicModule } from '@ionic/angular';
import { RegistrationPageRoutingModule } from './registration-routing.module';
import { RegistrationPage } from './registration.page';

@NgModule({
  imports: [
    CommonModule,
    FormsModule,
    ReactiveFormsModule, // Import ReactiveFormsModule for form handling
    IonicModule,
    RegistrationPageRoutingModule
  ],
  declarations: [RegistrationPage]
})
export class RegistrationPageModule {}

// src/app/registration/registration-routing.module.ts

// This module sets up the routing for the registration page, defining the path and component.
// Questions:
// 1. What does the `path: ''` route in `RegistrationPageRoutingModule` signify?

import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { RegistrationPage } from './registration.page';

const routes: Routes = [
  {
    path: '',
    component: RegistrationPage // Set RegistrationPage as the component for this route
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)], // Configure the child routes
  exports: [RouterModule],
})
export class RegistrationPageRoutingModule {}

// src/app/registration/registration.page.ts

// This component handles the logic for the registration page, including form initialization and submission handling.
// Questions:

// 1. What role does the FormBuilder play in the RegistrationPage component?
// 2. How does the onSubmit method handle form submission and validation?
// 3. What is the purpose of the validateAllFormFields method in the component?
// 4. Why is it important to check if a control is null before calling markAsTouched in the validateAllFormFields method?

import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-registration',
  templateUrl: './registration.page.html',
  styleUrls: ['./registration.page.scss'],
})
export class RegistrationPage implements OnInit {
  registrationForm: FormGroup; // Define the form group for the registration form

  constructor(private formBuilder: FormBuilder) {
    // Initialize the form with form controls and validators
    this.registrationForm = this.formBuilder.group({
      email: ['', [Validators.required, Validators.email]], // Email field with validators
      password: ['', [Validators.required, Validators.minLength(6)]], // Password field with validators
      passwordConfirmation: ['', [Validators.required, Validators.minLength(6)]] // Password confirmation field with validators
    });
  }

  ngOnInit() {}

  // Handle form submission
  onSubmit() {
    if (this.registrationForm.valid) {
      console.log(this.registrationForm.value); // Log form values to the console
      // Here you would send the form data to your Devise backend
    } else {
      this.validateAllFormFields(this.registrationForm); // Validate all fields to show validation errors
    }
  }

  // Validate all form fields
  validateAllFormFields(formGroup: FormGroup) {
    Object.keys(formGroup.controls).forEach(field => {
      const control = formGroup.get(field);
      if (control) {
        if (control instanceof FormGroup) {
          this.validateAllFormFields(control);
        } else {
          control.markAsTouched({ onlySelf: true });
        }
      }
    });
  }
}


// src/app/registration/registration.page.html

// This template defines the registration form structure and binds it to the form group defined in the component.
// Questions:
// 1. How do the formControlName attributes link to the form controls defined in the registration.page.ts file?
// 2. What is the purpose of the formGroup directive in the <form> tag?
// 3. Why are ion-note elements used in this template, and when do they appear?
// 4. How does the non-null assertion operator (!) help in accessing form control properties like touched and hasError?
// 5. Research other types of validation methods there are for angular?
// 6. Create a custom validation that shows error when passwords don't match

<ion-header>
  <ion-toolbar>
    <ion-title>Registration</ion-title>
  </ion-toolbar>
</ion-header>

<ion-content>
  <form [formGroup]="registrationForm" (ngSubmit)="onSubmit()">
    <ion-item>
      <ion-label position="floating">Email</ion-label>
      <ion-input type="email" formControlName="email"></ion-input>
    </ion-item>
    <ion-note *ngIf="registrationForm.get('email')!.touched && registrationForm.get('email')!.hasError('required')" color="danger">
      Email is required.
    </ion-note>
    <ion-note *ngIf="registrationForm.get('email')!.touched && registrationForm.get('email')!.hasError('email')" color="danger">
      Enter a valid email.
    </ion-note>

    <ion-item>
      <ion-label position="floating">Password</ion-label>
      <ion-input type="password" formControlName="password"></ion-input>
    </ion-item>
    <ion-note *ngIf="registrationForm.get('password')!.touched && registrationForm.get('password')!.hasError('required')" color="danger">
      Password is required.
    </ion-note>
    <ion-note *ngIf="registrationForm.get('password')!.touched && registrationForm.get('password')!.hasError('minlength')" color="danger">
      Password must be at least 6 characters long.
    </ion-note>

    <ion-item>
      <ion-label position="floating">Password Confirmation</ion-label>
      <ion-input type="password" formControlName="passwordConfirmation"></ion-input>
    </ion-item>
    <ion-note *ngIf="registrationForm.get('passwordConfirmation')!.touched && registrationForm.get('passwordConfirmation')!.hasError('required')" color="danger">
      Password confirmation is required.
    </ion-note>
    <ion-note *ngIf="registrationForm.get('passwordConfirmation')!.touched && registrationForm.get('passwordConfirmation')!.hasError('minlength')" color="danger">
      Password confirmation must be at least 6 characters long.
    </ion-note>

    <ion-button expand="full" type="submit">Register</ion-button>
  </form>
</ion-content>


// src/app/app-routing.module.ts

// This module sets up the app-wide routing configuration, including lazy loading the registration module.
// Questions:
// 1. How does lazy loading work for the registration module in `AppRoutingModule`?
// 2. What does the `redirectTo: 'home'` route do?

import { NgModule } from '@angular/core';
import { PreloadAllModules, RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  {
    path: 'registration',
    loadChildren: () => import('./registration/registration.module').then( m => m.RegistrationPageModule) // Lazy load the registration module
  },
];

...

// src/app/registration/registration.page.scss

// This file contains the styles for the registration page.
// Questions:
// 1. How do the styles in `registration.page.scss` affect the appearance of the registration page?
// 2. What is the impact of setting `--background` and `--color` for `ion-toolbar` in the SCSS file?

ion-header {
  --background: #f8f8f8; // Set background color for the header
}

ion-toolbar {
  --background: #3880ff; // Set background color for the toolbar
  --color: #fff; // Set text color for the toolbar
}

ion-title {
  font-weight: bold; // Set font weight for the title
}

ion-item {
  margin-top: 10px; // Add top margin to each item
}

ion-button {
  margin-top: 20px; // Add top margin to the button
}

ion-note {
  margin-top: 5px;
  margin-left: 16px;
}

// Run the app

ionic serve

// Add new field

// Questions:
// 1. Add new field to html page called `Full name`
// 2. Connect it to the page.ts file so it contains validation
// 3. Ensure validation display a message if left blank

// Run the app

ionic serve
```
