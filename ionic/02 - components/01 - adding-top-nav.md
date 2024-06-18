
### Generate a new component for the top navigation bar
```
ionic generate component components/top-nav


# Questions:
# 1. What files and folders are created when you generate a new component?
# 2. How is the difference between component and a page?
```

### src/app/components/top-nav/top-nav.component.ts
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-top-nav',
  templateUrl: './top-nav.component.html',
  styleUrls: ['./top-nav.component.scss'],
})
export class TopNavComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

# Questions:
# 1. What role does the `ngOnInit` method play in this component?
# 2. What is the role of this `component.ts` file?
```

### src/app/components/top-nav/top-nav.component.html
```
<ion-header>
  <ion-toolbar>
    <ion-buttons slot="start">
      <ion-button>
        <ion-icon slot="icon-only" name="menu"></ion-icon>
      </ion-button>
    </ion-buttons>
    <ion-title>
      <img src="assets/logo.png" alt="App Logo" style="height: 30px;">
    </ion-title>
    <ion-buttons slot="end">
      <ion-button>
        <ion-icon slot="icon-only" name="notifications-outline"></ion-icon>
      </ion-button>
      <ion-button>
        <ion-icon slot="icon-only" name="person-circle-outline"></ion-icon>
      </ion-button>
    </ion-buttons>
  </ion-toolbar>
</ion-header>

# Questions:
# 1. How can you customize this template to display different icons or elements?
# 2. What is the role of Ionic components in this template?
# 3. Where would you save the image so it works in ionic?
```

### src/app/components/top-nav/top-nav.component.scss

```
ion-header {
  --background: #f8f8f8;
}

ion-toolbar {
  --background: #3880ff;
  --color: #fff;
}

ion-title img {
  max-height: 30px;
}

ion-buttons {
  --color: #fff;
}


# Questions:
#  1. How can you customize these styles to change the look of the navigation bar?
```

### src/app/app.component.html
```
# Add the top navigation bar to the main app component, inside: <ion-app>
<app-top-nav></app-top-nav>
<ion-router-outlet></ion-router-outlet>

# Questions:
# 1. How does adding <app-top-nav> to this template affect app layout?
# 2. What is the purpose of <ion-router-outlet> in this context?
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
import { TopNavComponent } from './components/top-nav/top-nav.component'; # <<<< imported here

@NgModule({
  declarations: [AppComponent, TopNavComponent], # <<<< declared here
  imports: [BrowserModule, IonicModule.forRoot(), AppRoutingModule],
  providers: [{ provide: RouteReuseStrategy, useClass: IonicRouteStrategy }],
  bootstrap: [AppComponent],
})
export class AppModule {}

# Questions:
# 1. What exact change did we make to this file app/app.module.ts
# 2. Why did we add TopNavComponent to declaration?
# 3. What error would you get without TopNavComponent when you run ionic serve?
```

### Run the app to see the top navigation bar
```
ionic serve
```
