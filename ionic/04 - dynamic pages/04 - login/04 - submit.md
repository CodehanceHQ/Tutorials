# Replace signin.page.ts
##### Now we complete setting up signin.page.ts to use SigninService
```
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { Router } from '@angular/router';
import { SigninService } from '../../../services/auth/signin.service';
import { catchError, tap } from 'rxjs/operators';
import { of } from 'rxjs';

@Component({
  selector: 'app-login',
  templateUrl: './login.page.html',
  styleUrls: ['./login.page.scss'],
})
export class LoginPage implements OnInit {
  backendErrors: string[] = [];
  signInForm: FormGroup;

  constructor(
    private formBuilder: FormBuilder, 
    private signinService: SigninService,
    private router: Router,
  ) {
    this.signInForm = this.formBuilder.group({
      email: ['', [Validators.required, Validators.email]],
      password: ['', Validators.required],
    });
  }

  ngOnInit() {
  }

  onSubmit() {
    if (this.signInForm.valid) {
      this.signinService.signIn(this.signInForm.value).pipe(
        tap(response => {
          console.log('response': response);
          if (response.errors && response.errors.length > 0) {
            this.backendErrors = response.errors;
          } else {
            this.router.navigate(['/home']);
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

### Inpect console
```
visit: http://localhost:8100/signin

# login
Login with a real user's details and due to the console log we placed in onSubmit you should notice the response we receive from backend.
```

### Save Token
##### update login.page.ts replace onSubmit():
```
onSubmit() {
  if (this.signInForm.valid) {
    this.signinService.signIn(this.signInForm.value).pipe(
      tap(response => {
        console.log('response', response);
        if (response.errors && response.errors.length > 0) {
          this.backendErrors = response.errors;
        } else {
          // The token is automatically stored in the SigninService
          this.signInForm.reset(); // Clear form data
          this.router.navigate(['/home']);
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
```