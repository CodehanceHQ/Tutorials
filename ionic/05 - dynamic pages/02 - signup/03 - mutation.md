### Create Sign Up Mutation
```
# create file
mkdir -p src/app/graphql/mutations
touch src/app/graphql/mutations/signUp.mutation.ts
```

### signUp.mutation.ts
```
import gql from 'graphql-tag';

export const SIGN_UP = gql`
  mutation SignUp($input: SignUpInput!) {
    signUp(input: $input) {
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