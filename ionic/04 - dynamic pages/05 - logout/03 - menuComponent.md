### side-menu.component.ts
```
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';
import { LogoutService } from '../services/auth/logout.service';

@Component({
  selector: 'app-side-menu',
  templateUrl: './side-menu.component.html',
  styleUrls: ['./side-menu.component.scss'],
})
export class SideMenuComponent implements OnInit {

  constructor(
    private logoutService: LogoutService, 
    private router: Router
  ) { }

  ngOnInit() {}

  async onLogout() {
    await this.logoutService.logout();
    this.router.navigate(['/signin']);
  }
}
```