### Generate a new page for the login
```
ionic generate page pages/login
```

### src/app/pages/login/login.page.ts
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

### src/app/pages/login/login.page.html
```
# This template defines the structure of the login page.

<ion-header>
  <ion-toolbar>
    <ion-title>Login</ion-title>
  </ion-toolbar>
</ion-header>

<ion-content>
  <ion-grid>
    <ion-row class="ion-justify-content-center">
      <ion-col size-md="6" size-lg="4">
        <form (submit)="onLogin()">
          <ion-item>
            <ion-label position="floating">Email</ion-label>
            <ion-input type="email" required></ion-input>
          </ion-item>
          <ion-item>
            <ion-label position="floating">Password</ion-label>
            <ion-input type="password" required></ion-input>
          </ion-item>
          <ion-button expand="full" type="submit">Login</ion-button>
        </form>
        <div class="link-container">
          <div class="link-item">
            <p class="link-question">Don't have an account?</p>
            <ion-item lines="none" class="no-padding">
              <ion-label>
                <a href="/registration" class="auth-link">Click here</a>
              </ion-label>
            </ion-item>
          </div>
          <div class="link-item">
            <p class="link-question">Forgot your password?</p>
            <ion-item lines="none" class="no-padding">
              <ion-label>
                <a href="/forgot-password" class="auth-link">Click here</a>
              </ion-label>
            </ion-item>
          </div>
        </div>
      </ion-col>
    </ion-row>
  </ion-grid>
</ion-content>

# Questions:
# 1. Explain `ion-grid`, `ion-row`, and `ion-col` components in this template?
```

### src/app/pages/login/login.page.scss
```
ion-header {
  --background: #f8f8f8;
}

ion-toolbar {
  --background: #3880ff;
  --color: #fff;
}

form {
  ion-item {
    margin-bottom: 16px;
  }
}

ion-button {
  margin-top: 16px;
}

ion-label a {
  text-decoration: none;
  color: #3880ff;
}

.link-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

.link-item {
  display: flex;
  align-items: center;
  margin: 10px 0;
}

.link-question {
  font-size: 14px;
  color: #666;
  margin-right: 10px;
}

.auth-link {
  text-decoration: none;
  color: #3880ff;
  font-weight: bold;
  white-space: nowrap;
}

.no-padding {
  padding: 0;
}
```

### src/app/app-routing.module.ts
```
# Add below to right locations in app-routing.module.ts if not already there
# We want path: '' to redirect to login page.

{ path: '', redirectTo: 'login', pathMatch: 'full' },
{ path: 'login', loadChildren: () => import('./pages/login/login.module').then(m => m.LoginPageModule) },
```

### Run the app to see the login page
```
ionic serve
```