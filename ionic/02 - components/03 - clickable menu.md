
### src/app/components/top-nav/top-nav.component.ts
```
# Edit top-nav.component.ts import MenuController, add openMenu() and update the constructoor

import { Component, OnInit } from '@angular/core';
import { MenuController } from '@ionic/angular';

@Component({
  selector: 'app-top-nav',
  templateUrl: './top-nav.component.html',
  styleUrls: ['./top-nav.component.scss'],
})
export class TopNavComponent implements OnInit {

  constructor(private menu: MenuController) { }

  ngOnInit() {
  }

  openMenu() {
    this.menu.open('first');
  }
}

# Questions:
# 1. What change did we add to the top-nav.component.ts?
# 2. Where was MenuController imported from?
# 3. Explain changes made to the constructor
# 4. Explain openMenu() function
```

### src/app/components/top-nav/top-nav.component.html
```
# Edit icon-button for menu icon as below

<ion-button (click)="openMenu()">
  <ion-icon slot="icon-only" name="menu"></ion-icon>
</ion-button>

# Questions:
# 1. What change did we add to the top-nav.component.html?
# 1. Where is the openMenu function defined?
```

### Generate the side-menu
```
ionic generate component components/side-menu
```

### side-menu.component.html
```
<ion-menu side="start" menuId="first" contentId="main-content">
  <ion-header>
    <ion-toolbar>
      <ion-title>Menu</ion-title>
    </ion-toolbar>
  </ion-header>
  <ion-content>
    <ion-list>
      <ion-item>
        <ion-label>Profile</ion-label>
      </ion-item>
      <ion-item>
        <ion-label>Settings</ion-label>
      </ion-item>
      <ion-item>
        <ion-label>Logout</ion-label>
      </ion-item>
    </ion-list>
  </ion-content>
</ion-menu>

# Questions:
# 1. How does the <ion-menu> component integrate with the rest of the application layout?
# 2. What is the significance of the contentId attribute in <ion-menu>?
# 2. How is menuId="first" used by the top-nav.component.ts?
```

### side-menu.component.scss
```
ion-header {
  --background: #f8f8f8;
}

ion-toolbar {
  --background: #3880ff;
  --color: #fff;
}
```

### src/app/app.module.ts
```
# Update to import and use SideMenuComponent
...
import { SideMenuComponent } from './side-menu/side-menu.component';

@NgModule({
  declarations: [..., SideMenuComponent],
  ...
})
export class AppModule {}
```

### src/app/app.component.html
```
<ion-app>
  <app-top-nav></app-top-nav>
  <app-side-menu></app-side-menu>
  <div id="main-content">
    <ion-router-outlet></ion-router-outlet>
  </div>
  <app-bottom-nav></app-bottom-nav>
</ion-app>

# Questions:
# 1. Why do we have main-content above?
# 2. Where is main-content refrenced from
```

### Run the app to see the top navigation bar with the menu
```
ionic serve
```
