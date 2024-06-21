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

@Injectable({
  providedIn: 'root',
})
export class SigninService {
  constructor(
    private apollo: Apollo,
    private loggingService: LoggingService // Inject the LoggingService
  ) {}

  signIn(input: any): Observable<any> {
    return this.apollo.mutate({
      mutation: SIGN_IN,
      variables: {
        input: input,
      },
    }).pipe(
      map((result: any) => result.data.signIn),
      catchError(error => {
        this.loggingService.logError('Failed to sign in', error); // Log the error
        return throwError(() => new Error('Sign in failed')); // Rethrow the error
      })
    );
  }
}
```