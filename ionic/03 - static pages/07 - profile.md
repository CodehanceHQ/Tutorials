### Update User page
```
ionic generate page pages/profile
```

### Update the html page
```
<ion-content>
  <div class="custom-header">
    <h1>Profile</h1>
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

    <ion-button expand="full" type="submit">Update</ion-button>
  </form>

  <ion-row class="ion-justify-content-center ion-margin-top">
    <ion-col size="auto">
      <a href="/change-password">Change password</a>
    </ion-col>
  </ion-row>
</ion-content>
```