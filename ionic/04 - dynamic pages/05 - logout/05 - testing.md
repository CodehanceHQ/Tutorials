### Create Logout test
```
touch cypress/e2e/auth/logout.cy.ts
```

### Create sigin helper function
```
mkdir -p cypress/support
touch cypress/support/helpers.ts
```

### cypress/support/helpers.js
```
// Define TypeScript interfaces for the fixture data

import { 
  SignInData, 
  SuccessResponse,
} from '../fixtures/signin/types';

// Define global variables with explicit types
let signInData: SignInData;
let successResponse: SuccessResponse;

export function login() {
  cy.fixture('signin/data').then((data) => {
    signInData = data as SignInData;
  });

  cy.fixture('signin/success_response').then((data) => {
    successResponse = data as SuccessResponse;
  });

  // Mock the GraphQL login request
  cy.intercept('POST', '**/graphql', (req) => {
    if (req.body.operationName === 'Login') {
      req.reply(successResponse);
    }
  });

  // Simulate user login by setting a cookie or localStorage item
  cy.visit('/login');

  // Fill out the form using the fixture data
  cy.get('ion-input[formControlName="email"] input').type(signInData.email);
  cy.get('ion-input[formControlName="password"] input').type(signInData.password);
  
  // Submit the form
  cy.get('ion-button[type="submit"]').click();
}
```


### Signin.cy.ts
```
# cypress/e2e/auth/signup.cy.ts

// Define global variables with explicit types
let signUpData: SignUpData;
let successResponse: SuccessResponse;
let errorResponse: ErrorResponse;

describe('Signup Page', () => {

  it('should allow a user to signup', () => {
    // Mock the GraphQL signup request
    cy.intercept('POST', '**/graphql', (req) => {
      if (req.body.operationName === 'SignUp') {
        req.reply(successResponse);
      }
    }).as('signUpRequest');

    // Visit the signup page
    cy.visit('/signup');

    // Fill out the form using the fixture data
    cy.get('ion-input[formControlName="fullName"] input').type(signUpData.fullName);
    cy.get('ion-input[formControlName="email"] input').type(signUpData.email);
    cy.get('ion-input[formControlName="telephone"] input').type(signUpData.telephone);
    cy.get('ion-input[formControlName="password"] input').type(signUpData.password);
    cy.get('ion-input[formControlName="confirmPassword"] input').type(signUpData.confirmPassword);

    // Submit the form
    cy.get('ion-button[type="submit"]').click();

    // Wait for the mocked request to complete
    cy.wait('@signUpRequest');

    // Verify that the URL includes '/login'
    cy.url().should('include', '/login');
  });

  it('should handle signup error', () => {
    // Mock the GraphQL signup request
    cy.intercept('POST', '**/graphql', (req) => {
      if (req.body.operationName === 'SignUp') {
        req.reply(errorResponse);
      }
    }).as('signUpRequest');

    // Visit the signup page
    cy.visit('/signup');

    // Fill out the form using the fixture data
    cy.get('ion-input[formControlName="fullName"] input').type(signUpData.fullName);
    cy.get('ion-input[formControlName="email"] input').type(signUpData.email);
    cy.get('ion-input[formControlName="telephone"] input').type(signUpData.telephone);
    cy.get('ion-input[formControlName="password"] input').type(signUpData.password);
    cy.get('ion-input[formControlName="confirmPassword"] input').type(signUpData.confirmPassword);

    // Submit the form
    cy.get('ion-button[type="submit"]').click();

    // Wait for the mocked request to complete
    cy.wait('@signUpRequest');

    // Verify that the URL does not include '/login'
    cy.url().should('not.include', '/login');
    // Verify that the error message is displayed
    cy.contains('Email has already been taken').should('be.visible');
  });
});
```

### Run headless
##### Make sure your ionic app is running at in another tab
##### at port 8100
```
npm run cypress:run

# Question:
# 1: What does running cypress in headless mode mean?
# 2: Where did we define `cypess:run`?
# 3: Can you change that to what ever you want?
```

### Run in browser
##### Follow steps to run the e2e test, this will show
##### all green.
```
npm run cypress:open

# to stop the test
ctr + c 
```