### Forgot Password page
```
ionic generate page pages/auth/forgot_password
```

### Update the html page
```
<ion-content>
  <div class="custom-header">
    <h1>Forgot Password</h1>
  </div>

  <form>
    <ion-item>
      <ion-label position="floating">Email</ion-label>
      <ion-input type="email"></ion-input>
    </ion-item>

    <ion-button expand="full" type="submit">Send OTP</ion-button>
  </form>

  <ion-row class="ion-justify-content-center ion-margin-top">
    <ion-col size="auto">
      <a href="/signup">Don't have an account?</a>
    </ion-col>
    <ion-col size="auto">
      /
    </ion-col>
    <ion-col size="auto">
      <a href="/login">Login</a>
    </ion-col>
  </ion-row>
</ion-content>
```