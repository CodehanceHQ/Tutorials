### Reset Password page
```
ionic generate page pages/auth/reset_password
```

### Update reset-password.page.ts
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-reset-password',
  templateUrl: './reset-password.page.html',
  styleUrls: ['./reset-password.page.scss'],
})
export class ResetPasswordPage implements OnInit {

  constructor() { }

  ngOnInit() { }

  moveFocus(event: any, nextElement: any) {
    if (event.target.value.length === 1) {
      nextElement.setFocus();
    }
  }
}
```

### Update the html page
```
<ion-content>
  <div class="custom-header">
    <h1>Reset Password</h1>
  </div>

  <form>
    <ion-item>
      <ion-label position="floating">OTP ( sent to your email )</ion-label>
      <div class="otp-inputs">
        <ion-input #otp1 type="text" maxlength="1" class="otp-box" (keyup)="moveFocus($event, otp2)"></ion-input>
        <ion-input #otp2 type="text" maxlength="1" class="otp-box" (keyup)="moveFocus($event, otp3)"></ion-input>
        <ion-input #otp3 type="text" maxlength="1" class="otp-box" (keyup)="moveFocus($event, otp4)"></ion-input>
        <ion-input #otp4 type="text" maxlength="1" class="otp-box"></ion-input>
      </div>
    </ion-item>

    <ion-item>
      <ion-label position="floating">New Password</ion-label>
      <ion-input type="password"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label position="floating">Confirm New Password</ion-label>
      <ion-input type="password"></ion-input>
    </ion-item>

    <ion-button expand="full" type="submit">Reset Password</ion-button>
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

### Update the scss
```
.otp-inputs {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 30px;
}
.otp-box {
  width: 50px;
  text-align: center;
  border: 1px solid #000;
  border-radius: 10%;
}
```