### signup.module.ts
```
# ==================================================
# We import `ReactiveFormsModule` as well as include
# it in the imports array.
# ==================================================

...
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [
    ...
    ReactiveFormsModule
  ],
  ...
})
...

# Questions:
# 1: What is ReactiveFormsModule?
# 2: What will it provide us with?
# 3: Why do we add it to signup.module.ts?
```

### sign-up.page.ts
```
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-signup',
  templateUrl: './signup.page.html',
  styleUrls: ['./signup.page.scss'],
})
export class SignupPage implements OnInit {
  signUpForm: FormGroup;

  constructor(private formBuilder: FormBuilder) {
    this.signUpForm = this.formBuilder.group({
      fullName: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]],
      telephone: ['', [Validators.required, Validators.pattern('^[0-9]+$')]],
      password: ['', Validators.required],
      confirmPassword: ['', Validators.required]
    });
  }

  ngOnInit() {
  }

  onSubmit() {
    if (this.signUpForm.valid) {
      console.log(this.signUpForm.value);
      // Handle the form submission logic here
    }
  }
}

# Questions:
# 1: What did we import from `@angular/forms`?
# 2: What does each of those give us?
# 3: Why do we include formBuilder in the contructor?
# 4: What is happening within constructor?
# 5: What does `this` signify in `this.signUpForm` above?
# 6: Where will `onSubmit` be called from?
```

### sign-up.page.html
```
# ==================================================
# Replace the html with below
# Notce the new changes, ref. to formGroup, added
# onSubmit() each form input now has formControlName
# ==================================================

<ion-content>
  <div class="custom-header">
    <h1>Sign Up</h1>
  </div>

  <form [formGroup]="signUpForm" (ngSubmit)="onSubmit()">
    <ion-item>
      <ion-label position="floating">Full Name</ion-label>
      <ion-input type="text" formControlName="fullName"></ion-input>
    </ion-item> 

    <ion-item>
      <ion-label position="floating">Email</ion-label>
      <ion-input type="email" formControlName="email"></ion-input>
    </ion-item>
    <div *ngIf="signUpForm.get('email')?.hasError('email') && signUpForm.get('email')?.touched">
      <p class="error-message">Please enter a valid email address</p>
    </div>

    <ion-item>
      <ion-label position="floating">Telephone Number</ion-label>
      <ion-input type="tel" formControlName="telephone"></ion-input>
    </ion-item>
    <div *ngIf="signUpForm.get('telephone')?.hasError('pattern') && signUpForm.get('telephone')?.touched">
      <p class="error-message">Please enter a valid telephone number</p>
    </div>    

    <ion-item>
      <ion-label position="floating">Password</ion-label>
      <ion-input type="password" formControlName="password"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label position="floating">Confirm Password</ion-label>
      <ion-input type="password" formControlName="confirmPassword"></ion-input>
    </ion-item>

    <ion-button expand="full" type="submit" [disabled]="signUpForm.invalid">Register</ion-button>
  </form>

  <ion-row class="ion-justify-content-center ion-margin-top">
    <ion-col size="auto">
      <a href="/login">Have an account?</a>
    </ion-col>
    <ion-col size="auto">
      /
    </ion-col>
    <ion-col size="auto">
      <a href="/forgot-password">Forgot password?</a>
    </ion-col>
  </ion-row>
</ion-content>

# Questions:
# 1: What is the role of `[formGroup]="signUpForm"`
# 2: What component does the form data go to?
# 3: What does `[disabled]="signUpForm.invalid"` do?
# 4: What is `*ngIf` used above?
# 5: Where where is telephone in here `signUpForm.get('telephone')` defined?
```