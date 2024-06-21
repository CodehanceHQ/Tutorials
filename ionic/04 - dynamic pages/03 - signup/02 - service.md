### Create Sign Up Service
```
# create file
mkdir -p src/app/services/auth
touch src/app/services/auth/signup.service.ts
```

# Content for signup.service.ts
```
import { Injectable } from '@angular/core';
import { Apollo } from 'apollo-angular';
import { Observable, throwError } from 'rxjs';
import { map, catchError } from 'rxjs/operators';
import { SIGN_UP } from '../../graphql/mutations/signUp.mutation';
import { LoggingService } from '../logs/logging.service';

@Injectable({
  providedIn: 'root',
})
export class SignupService {
  constructor(
    private apollo: Apollo,
    private loggingService: LoggingService // Inject the LoggingService
  ) {}

  signUp(input: any): Observable<any> {
    return this.apollo.mutate({
      mutation: SIGN_UP,
      variables: {
        input: input,
      },
    }).pipe(
      map((result: any) => result.data.signUp),
      catchError(error => {
        this.loggingService.logError('Failed to sign up', error); // Log the error
        return throwError(() => new Error('Sign up failed')); // Rethrow the error
      })
    );
  }
}

# Questions:
# 1: Where do we plan to import the sign up mutation from?
# 2: What does @injectable providedIn: 'root' achieve?
# 3: What does `private apollo: Apollo` achieve above?
# 4: How many methods does our class have?
# 5: What method from apollo did we use?
# 6: Why did we use `Observable` type?
# 7: What argument will be passedd to signUp?
```