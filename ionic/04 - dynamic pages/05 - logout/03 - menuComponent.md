### side-menu.component.ts
```
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';
import { LogoutService } from '../services/auth/logout.service';
import { MenuController } from '@ionic/angular';

@Component({
  selector: 'app-side-menu',
  templateUrl: './side-menu.component.html',
  styleUrls: ['./side-menu.component.scss'],
})
export class SideMenuComponent implements OnInit {

  constructor(
    private logoutService: LogoutService, 
    private router: Router,
    private menu: MenuController
  ) { }

  ngOnInit() {}

  async onLogout() {
    await this.logoutService.logout();
    // close the menu
    this.menu.close('first');
    this.router.navigate(['/signin']);
  }
}
```