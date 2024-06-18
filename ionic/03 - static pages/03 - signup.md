### Signup page
```
ionic generate page pages/auth/signup
```

### Update the html
```
<ion-content>
  <div class="custom-header">
    <h1>Sign Up</h1>
  </div>

  <form>
    <ion-item>
      <ion-label position="floating">Full Name</ion-label>
      <ion-input type="text"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label position="floating">Email</ion-label>
      <ion-input type="email"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label position="floating">Telephone Number</ion-label>
      <ion-input type="tel"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label position="floating">Password</ion-label>
      <ion-input type="password"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label position="floating">Confirm Password</ion-label>
      <ion-input type="password"></ion-input>
    </ion-item>

    <ion-button expand="full" type="submit">Register</ion-button>
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
```