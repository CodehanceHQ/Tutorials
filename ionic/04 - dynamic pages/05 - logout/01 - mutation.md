### Create Sign In Mutation
```
# create file
touch src/app/graphql/mutations/logout.mutation.ts
```

### logout.mutation.ts
```
import gql from 'graphql-tag';

export const LOGOUT = gql`
  mutation Logout($input: LogoutInput!) {
    logout(input: $input) {
      data {
        id
        fullName
        email
        telephone
      }
      message
      errors
      httpStatus
    }
  }
`;
```