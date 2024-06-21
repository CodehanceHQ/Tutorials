### Create Sign In Mutation
```
# create file
mkdir -p src/app/graphql/mutations
touch src/app/graphql/mutations/signIn.mutation.ts
```

### signIn.mutation.ts
```
import gql from 'graphql-tag';

export const SIGN_IN = gql`
  mutation SignIn($input: SignInInput!) {
    signIn(input: $input) {
      data {
        id
        fullName
        email
        telephone
      }
      message
      errors
      httpStatus
      token
    }
  }
`;
```