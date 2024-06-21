### signin.module.ts
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

### signin.page.ts
```
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-signin',
  templateUrl: './signin.page.html',
  styleUrls: ['./signin.page.scss'],
})
export class SigninPage implements OnInit {
  backendErrors: string[] = [];
  signinForm: FormGroup;

  constructor(private formBuilder: FormBuilder) {
    this.signinForm = this.formBuilder.group({
      email: ['', [Validators.required, Validators.email]],
      password: ['', Validators.required],
    });
  }

  ngOnInit() {
  }

  onSubmit() {
    if (this.signinForm.valid) {
      console.log('Form submitted');
    }
  }
}
```

### signin.page.html
```
# ==================================================
# Replace the html with below
# Notce the new changes, ref. to formGroup, added
# onSubmit() each form input now has formControlName
# ==================================================

<ion-content>
  <div class="custom-header">
    <h1>Login</h1>
  </div>

  <div *ngIf="backendErrors.length > 0">
    <ul>
      <li *ngFor="let error of backendErrors">{{ error }}</li>
    </ul>
  </div>

  <form [formGroup]="signInForm" (ngSubmit)="onSubmit()">
    <ion-item>
      <ion-label position="floating">Email</ion-label>
      <ion-input type="email" formControlName="email"></ion-input>
    </ion-item>  

    <ion-item>
      <ion-label position="floating">Password</ion-label>
      <ion-input type="password" formControlName="password"></ion-input>
    </ion-item>

    <ion-button expand="full" type="submit" [disabled]="signInForm.invalid">Login</ion-button>
  </form>

  <ion-row class="ion-justify-content-center ion-margin-top">
    <ion-col size="auto">
      <a href="/signup">Don't have an account?</a>
    </ion-col>
    <ion-col size="auto">
      /
    </ion-col>
    <ion-col size="auto">
      <a href="/forgot-password">Forgot password?</a>
    </ion-col>
  </ion-row>
</ion-content>
```

### submit form
```
http://localhost:8102/signin

# fill in the form correctly and inspect element on page to view result.

# data is logged by signin.page.ts:

user console log to view output.
```