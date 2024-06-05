```
// Generate a new component for the bottom navigation bar (footer)
ionic generate component bottom-nav

// src/app/bottom-nav/bottom-nav.component.ts

// This component handles the logic for the bottom navigation bar.
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

// Questions:
// 1. In what scenarios might you prefer to use lifecycle hooks like `ngOnInit` over constructor logic?
// 2. How would you handle dynamic updates to the navigation items based on user role or permissions in this component?

// src/app/bottom-nav/bottom-nav.component.html

<!-- This template defines the structure of the bottom navigation bar (tabs). -->
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

<!-- Questions: -->
<!-- 1. What is the role of the `ion-tabs` and `ion-tab-bar` elements in this template? -->
<!-- 2. is <ion-tab-button> component we created on the fly or is it part of angular core? -->
<!-- 3. Where can you get a list of available components? as well as third party components? -->


// src/app/bottom-nav/bottom-nav.component.scss

/* This file contains the styles for the bottom navigation bar. */
ion-tab-bar {
  --background: #f8f8f8; /* Set background color for the tab bar */
}

ion-tab-button {
  --color-selected: #3880ff; /* Set color for selected tab */
  --color: #8c8c8c; /* Set color for unselected tabs */
}

/* Questions: */
/* 1. How do these styles impact the appearance and usability of the bottom navigation bar? */
/* 2. What considerations would you take into account when theming the navigation bar to ensure accessibility and responsiveness? */

// src/app/app.component.html

<!-- Add the bottom navigation bar to the main app component. -->
<ion-app>
  <app-top-nav></app-top-nav>
  <ion-router-outlet></ion-router-outlet>
  <app-bottom-nav></app-bottom-nav>
</ion-app>

<!-- Questions: -->
<!-- 1. What is the impact of adding multiple custom components like `<app-top-nav>` and `<app-bottom-nav>` to the root component? -->

// src/app/app.module.ts

// Import the new component and declare it in the app module.
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { RouteReuseStrategy } from '@angular/router';
import { IonicModule, IonicRouteStrategy } from '@ionic/angular';
import { AppComponent } from './app.component';
import { AppRoutingModule } from './app-routing.module';
import { TopNavComponent } from './top-nav/top-nav.component';
import { BottomNavComponent } from './bottom-nav/bottom-nav.component';

@NgModule({
  declarations: [AppComponent, TopNavComponent, BottomNavComponent],
  imports: [BrowserModule, IonicModule.forRoot(), AppRoutingModule],
  providers: [{ provide: RouteReuseStrategy, useClass: IonicRouteStrategy }],
  bootstrap: [AppComponent],
})
export class AppModule {}

// Questions:
// 1. Why is it necessary to declare both `TopNavComponent` and `BottomNavComponent` in the `AppModule`?

// Run the app to see the bottom navigation bar in action
ionic serve
```
