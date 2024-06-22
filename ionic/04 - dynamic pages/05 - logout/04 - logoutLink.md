### Intro
Logout means sending logout mutation request to backend to add user token to denylist and then frontend deletes the token from storage.

### Logout Link
We are not posting a form, but we do have to click on link to request for a logout action.

Here we update logout link by surrouding it with onLogout click function.
```
# side-menu.component.html

<ion-item (click)="onLogout()">
  <ion-label>Logout</ion-label>
</ion-item>

# Questions:
# 1: Where do we have to define onLogout() function?
```

### Update side menu-component.ts
We have to create the onLogout() function in the component ts file.
1) We are going to import logout service ( yet to create )
2) Inject it into the constructor
3) Define onLogout() within our class
```
# side-menu component.ts

import { Component, OnInit } from '@angular/core';
import { LogoutService } from '../services/auth/logout.service';

@Component({
  selector: 'app-side-menu',
  templateUrl: './side-menu.component.html',
  styleUrls: ['./side-menu.component.scss'],
})
export class SideMenuComponent implements OnInit {

  constructor(private logoutService: LogoutService) { }

  ngOnInit() {}

  onLogout() {
    this.logoutService.logout();
  }
}
```