### Generate a new page for the login
```
ionic generate page pages/auth/login
```

### src/app/pages/auth/login/login.page.ts
```
# This page handles the logic for the login.
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-login',
  templateUrl: './login.page.html',
  styleUrls: ['./login.page.scss'],
})
export class LoginPage implements OnInit {

  constructor() { }

  ngOnInit() {
  }

  onLogin() {
    // Logic for login goes here
  }
}

# Questions:
# 1. What functionality would be implemented in the `onLogin` method later?
```

### src/app/pages/auth/login/login.page.html
```
# This template defines the structure of the login page.

<ion-content>
  <div class="custom-header">
    <h1>Login</h1>
  </div>
  
  <form>
    <ion-item>
      <ion-label position="floating">Email</ion-label>
      <ion-input type="email"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label position="floating">Password</ion-label>
      <ion-input type="password"></ion-input>
    </ion-item>

    <ion-button expand="full" type="submit">Login</ion-button>
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

### src/glabal.scss
##### Add to bottom of file
```
ion-content {
  --padding-top: 56px; /* Adjust based on the height of your headers */
}

.custom-header {
  background-color: rgba(168, 171, 171, 0.813);
  color: white;
  padding: 5px;
  text-align: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

  h1 {
    margin: 0;
  }
}

.error-message {
  color: red;
  margin-left: 15px;
}
```

### src/app/app-routing.module.ts
```
# Add below to right locations in app-routing.module.ts if not already there
# We want path: '' to redirect to login page.

{ path: '', redirectTo: 'login', pathMatch: 'full' },
{ path: 'login', loadChildren: () => import('./pages/auth/login/login.module').then(m => m.LoginPageModule) },
```

### Run the app to see the login page
```
ionic serve
```