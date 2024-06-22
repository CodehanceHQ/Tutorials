### Create Login Service
```
# create file
touch src/app/services/auth/logout.service.ts
```

### logout.service.ts
```
import { Injectable } from '@angular/core';
import { Storage } from '@ionic/storage-angular';
import { Apollo } from 'apollo-angular';
import { LOGOUT } from 'src/app/graphql/mutations/logout.mutation';
import { LoggingService } from '../logs/logging.service';
import { catchError, map } from 'rxjs/operators';
import { Observable, throwError, firstValueFrom } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class LogoutService {
  private tokenKey = 'authToken';

  constructor(
    private storage: Storage,
    private apollo: Apollo,
    private loggingService: LoggingService
  ) {}

  addToDenylist(): Observable<any> {
    return this.apollo.mutate({
      mutation: LOGOUT,
      variables: {
        input: {},
      },
    }).pipe(
      map((result: any) => {
        return result.data.logout;
      }),
      catchError(error => {
        this.loggingService.logError('Failed adding to deny list', error);
        return throwError(() => new Error('Logout failed'));
      })
    );
  }

  async logout(): Promise<any> {
    try {
      const result = await firstValueFrom(this.addToDenylist());
      await this.storage.remove(this.tokenKey);
      return result;
    } catch (error) {
      this.loggingService.logError('Failed to logout', error);
      throw new Error('Logout failed');
    }
  }
}
```