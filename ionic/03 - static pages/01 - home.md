### Generate a new page called home
```
ionic generate page pages/home


# Questions:
# 1. What does the `ionic generate page` command do?
# 2. What files and folders are created when you generate a new page?
```

### src/app/app-routing.module.ts
```
import { NgModule } from '@angular/core';
import { PreloadAllModules, RouterModule, Routes } from '@angular/router';

const routes: Routes = [
  {
    path: '',
    redirectTo: 'home', // Redirect to the home page by default
    pathMatch: 'full'
  },
  {
    path: 'home',
    loadChildren: () => import('./pages/home/home.module').then( m => m.HomePageModule) // Lazy load the home module
  }
];

@NgModule({
  imports: [
    RouterModule.forRoot(routes, { preloadingStrategy: PreloadAllModules }) // Configure the app routes with preloading strategy
  ],
  exports: [RouterModule]
})
export class AppRoutingModule {}

# Questions:
# 1. What does `RouterModule.forRoot` do?
# 2. What is lazy loading in Ionic
# 3. Why might you use lazy loading for routes in an Ionic application?
```

### src/app/pages/home/home.module.ts
```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { IonicModule } from '@ionic/angular';
import { HomePageRoutingModule } from './home-routing.module';
import { HomePage } from './home.page';

@NgModule({
  imports: [
    CommonModule,
    FormsModule,
    IonicModule,
    HomePageRoutingModule
  ],
  declarations: [HomePage]
})
export class HomePageModule {}

# Questions:
# 1. What role does the `HomePage` component play in this module?
# 2. How does `HomePageRoutingModule` integrate with this module?
# 3. What is the purpose of the `HomePageModule`?
# 4. Why is it important to import `CommonModule`, `FormsModule`, and `IonicModule`?
```

### src/app/pages/home/home-routing.module.ts
```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomePage } from './home.page';

const routes: Routes = [
  {
    path: '',
    component: HomePage // Set HomePage as the component for this route
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)], // Configure the child routes
  exports: [RouterModule],
})
export class HomePageRoutingModule {}

# Questions:
# 1. Why is `RouterModule.forChild` used in this context?
# 2. What is the significance of setting `HomePage` as the component for this route?
# 3. What does the `path: ''` route in `HomePageRoutingModule` signify?
```

### src/app/pages/home/home.page.ts
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-home',
  templateUrl: './home.page.html',
  styleUrls: ['./home.page.scss'],
})
export class HomePage implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}

# Questions:
# 1. What is the role of the `ngOnInit` method in this component?
# 2. How do you define additional logic or methods for this component?
```

### src/app/pages/home/home.page.html
```
<ion-header>
  <ion-toolbar>
    <ion-title>
      Home
    </ion-title>
  </ion-toolbar>
</ion-header>

<ion-content>
  <p>Welcome to the Home Page!</p>
</ion-content>

# Questions:
# 1. How can you customize this template to display different content?
# 2. What is the role of Ionic components in this template?
```

### src/app/pages/home/home.page.scss
```
ion-header {
  --background: #f8f8f8;
}

ion-toolbar {
  --background: #3880ff;
  --color: #fff;
}

ion-title {
  font-weight: bold;
}

ion-content {
  padding: 10px;
}

# Questions:
# 1. How do these styles affect the appearance of the home page?
# 2. How can you customize these styles to change the look of the page?
```

### Run the app
```
ionic serve
```