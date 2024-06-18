### Change Password page
```
ionic generate page pages/auth/change_password
```

### Update the html page
```
<ion-content>
  <div class="custom-header">
    <h1>Change Password</h1>
  </div>

  <form>
    <ion-item>
      <ion-label position="floating">Current Password</ion-label>
      <ion-input type="password"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label position="floating">New Password</ion-label>
      <ion-input type="password"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label position="floating">Confirm New Password</ion-label>
      <ion-input type="password"></ion-input>
    </ion-item>

    <ion-button expand="full" type="submit">Change Password</ion-button>
  </form>

  <ion-row class="ion-justify-content-center ion-margin-top">
    <ion-col size="auto">
      <a href="/">Back to home</a>
    </ion-col>
  </ion-row>
</ion-content>
```