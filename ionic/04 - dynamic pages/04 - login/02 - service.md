### Create Login Service
```
# create file
mkdir -p src/app/services/auth
touch src/app/services/auth/signin.service.ts
```

# Content for signin.service.ts
```
import { Injectable } from '@angular/core';
import { Apollo } from 'apollo-angular';
import { Observable, throwError } from 'rxjs';
import { map, catchError } from 'rxjs/operators';
import { SIGN_IN } from '../../graphql/mutations/signIn.mutation';
import { LoggingService } from '../logs/logging.service';
import { Storage } from '@ionic/storage-angular';

@Injectable({
  providedIn: 'root',
})
export class SigninService {
  private tokenKey = 'authToken';

  constructor(
    private apollo: Apollo,
    private loggingService: LoggingService,
    private storage: Storage
  ) {}

  signIn(input: any): Observable<any> {
    return this.apollo.mutate({
      mutation: SIGN_IN,
      variables: { input: input }
    }).pipe(
      map((result: any) => {
        const signInResult = result.data.signIn;
        if (signInResult.token) {
          this.storage.set(this.tokenKey, signInResult.token);
        }
        return signInResult;
      }),
      catchError(error => {
        this.loggingService.logError('Failed to sign in', error);
        return throwError(() => new Error('Sign in failed'));
      })
    );
  }
}
```