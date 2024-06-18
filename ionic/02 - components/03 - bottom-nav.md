
### Generate bottom nav
```
ionic generate component components/bottom-nav
```

### src/app/components/bottom-nav/bottom-nav.component.ts
```
# This component handles the logic for the bottom navigation bar.
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-bottom-nav',
  templateUrl: './bottom-nav.component.html',
  styleUrls: ['./bottom-nav.component.scss'],
})
export class BottomNavComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```

### src/app/components/bottom-nav/bottom-nav.component.html
```
<ion-tabs>
  <ion-tab-bar slot="bottom">
    <ion-tab-button tab="home">
      <ion-icon name="home-outline"></ion-icon>
      <ion-label>Home</ion-label>
    </ion-tab-button>
    <ion-tab-button tab="properties">
      <ion-icon name="list-outline"></ion-icon>
      <ion-label>Properties</ion-label>
    </ion-tab-button>
    <ion-tab-button tab="messages">
      <ion-icon name="chatbubble-outline"></ion-icon>
      <ion-label>Messages</ion-label>
    </ion-tab-button>
  </ion-tab-bar>
</ion-tabs>

# Questions:
# 1. What is the role of the `ion-tabs` and `ion-tab-bar` elements in this template?
# 2. is <ion-tab-button> component we created on the fly or is it part of angular core?
# 3. Where can you get a list of available components? as well as third party components?
```

### src/app/components/bottom-nav/bottom-nav.component.scss
```
ion-tab-bar {
  --background: #f8f8f8;
}

ion-tab-button {
  --color-selected: #3880ff;
  --color: #8c8c8c;
}
```

### src/app/app.component.html
```
# Add the bottom navigation bar to the main app component.

<ion-app>
  <app-top-nav></app-top-nav>
  <ion-router-outlet></ion-router-outlet>
  <app-bottom-nav></app-bottom-nav>
</ion-app>

# Questions:
# 1. What is the impact of adding multiple custom components like `<app-top-nav>` and `<app-bottom-nav>` to the root component?
```

### src/app/app.module.ts
```
# Import the new component and declare it in the app module.

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { RouteReuseStrategy } from '@angular/router';
import { IonicModule, IonicRouteStrategy } from '@ionic/angular';
import { AppComponent } from './app.component';
import { AppRoutingModule } from './app-routing.module';
import { TopNavComponent } from './top-nav/top-nav.component';
import { BottomNavComponent } from './components/bottom-nav/bottom-nav.component'; # <<<<< imported here

@NgModule({
  declarations: [AppComponent, TopNavComponent, BottomNavComponent], # <<<<< used here
  imports: [BrowserModule, IonicModule.forRoot(), AppRoutingModule],
  providers: [{ provide: RouteReuseStrategy, useClass: IonicRouteStrategy }],
  bootstrap: [AppComponent],
})
export class AppModule {}

# Questions:
# 1. Why is it necessary to declare both `TopNavComponent` and `BottomNavComponent` in the `AppModule`?
```

### Run the app to see the bottom navigation bar in action
```
ionic serve
```
