
### Step 1: Create Log Mutation
```
touch src/app/graphql/mutations/createLog.mutation.ts
```

### Step 2: createLog.mutation.ts
```
import gql from 'graphql-tag';

export const CREATE_LOG = gql`
  mutation CreateLog($input: CreateLogInput!) {
    createLog(input: $input) {
      status
      message
    }
  }
`;
```

### Step 3: Create logging service file
```
mkdir -p src/app/services/logs
touch src/app/services/logs/logging.service.ts
```

### Step 4: logging.service.ts
```
import { Injectable } from '@angular/core';
import { Apollo } from 'apollo-angular';
import { CREATE_LOG } from '../../graphql/mutations/createLog.mutation';
import { catchError } from 'rxjs/operators';
import { of } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class LoggingService {
  constructor(private apollo: Apollo) {}

  log(level: string, message: string, data: any = {}) {
    const timestamp = new Date().toISOString();

    const input = {
      level,
      message,
      timestamp,
      data,
    };

    return this.apollo.mutate({
      mutation: CREATE_LOG,
      variables: {
        input: input,
      },
    }).pipe(
      catchError(error => {
        console.error('Failed to send log to server', error);
        return of(null); // Return a null observable in case of error
      })
    ).subscribe({
      next: (response) => {
        if (response && response.data && response.data) {
          console.log('Log sent successfully:', response.data);
        } else {
          console.error('Log sending failed:', response);
        }
      },
      error: (error) => {
        console.error('Error sending log:', error);
      }
    });
  }

  logError(message: string, error: any) {
    const data = {
      stack: error.stack || null,
      name: error.name || null,
      message: error.message || null,
      ...error // Include any other properties of the error object
    };
    this.log('error', message, data);
  }
}
```