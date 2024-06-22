# Replace signup.page.ts
##### Now we complete setting up signup.page.ts to use SignupService
```
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { Router } from '@angular/router';
import { SignupService } from '../../../services/auth/signup.service';
import { catchError, tap } from 'rxjs/operators';
import { of } from 'rxjs';

@Component({
  selector: 'app-signup',
  templateUrl: './signup.page.html',
  styleUrls: ['./signup.page.scss'],
})
export class SignupPage implements OnInit {
  backendErrors: string[] = [];
  signUpForm: FormGroup;

  constructor(
    private formBuilder: FormBuilder, 
    private signupService: SignupService,
    private router: Router,
  ) {
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
      this.signupService.signUp(this.signUpForm.value).pipe(
        tap(response => {
          if (response.errors && response.errors.length > 0) {
            this.backendErrors = response.errors;
          } else {
            this.signUpForm.reset(); // Clear form data
            this.router.navigate(['/login']);
          }
        }),
        catchError(error => {
          // Error is logged at service level
          this.backendErrors.push('Oops! Something went wrong. Please try again later.');
          return of(null);
        })
      ).subscribe();
    }
  }
}
```
