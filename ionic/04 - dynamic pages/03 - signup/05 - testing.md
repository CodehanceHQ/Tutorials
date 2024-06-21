### Intro
```
Now we are going to write a test to protect the work we have done against unwanted changes.
```


### SignUp fixtures
```
mkdir -p cypress/fixtures/signup/
touch cypress/fixtures/signup/data.json
touch cypress/fixtures/signup/success_response.json
touch cypress/fixtures/signup/error_response.json
touch cypress/fixtures/signup/types.ts

# data.json
{
  "fullName": "John Doe",
  "email": "woric7929021@kernuo.com",
  "password": "PAssword@123",
  "confirmPassword": "PAssword@123",
  "telephone": "1234567890"
}

# success_response.json
{
  "data": {
    "signUp": {
      "data": {
        "id": "10",
        "fullName": "John Doe",
        "email": "woric79290213@kernuo.com",
        "telephone": "1234567890"
      },
      "message": "User created successfully",
      "errors": [],
      "httpStatus": 201
    }
  }
}

# error_response.json
{
  "data": {
    "signUp": {
      "data": null,
      "message": "User creation failed",
      "errors": [
        "Email has already been taken"
      ],
      "httpStatus": 422
    }
  }
}
```

### types.ts ( cypress/fixtures/signup/types.ts )
```
// Define TypeScript interfaces for the fixture data
export interface SignUpData {
  fullName: string;
  email: string;
  password: string;
  confirmPassword: string;
  telephone: string;
}

export interface SuccessResponse {
  data: {
    signUp: {
      data: {
        id: string;
        fullName: string;
        email: string;
        telephone: string;
      };
      message: string;
      errors: any[];
      httpStatus: number;
    };
  };
}

export interface ErrorResponse {
  data: {
    signUp: {
      data: null;
      message: string;
      errors: string[];
      httpStatus: number;
    };
  };
}

```


### Create Signup test
```
mkdir cypress/e2e/auth
touch cypress/e2e/auth/signup.cy.ts
```

### Signup.cy.ts
```
# cypress/e2e/auth/signup.cy.ts

// Define TypeScript interfaces for the fixture data

import { 
  SignUpData, 
  SuccessResponse, 
  ErrorResponse 
} from '../../fixtures/signup/types';

// Define global variables with explicit types
let signUpData: SignUpData;
let successResponse: SuccessResponse;
let errorResponse: ErrorResponse;

describe('Signup Page', () => {
  // Load fixtures before all tests
  before(() => {
    cy.fixture('signup/data').then((data) => {
      signUpData = data as SignUpData;
    });
    cy.fixture('signup/success_response').then((data) => {
      successResponse = data as SuccessResponse;
    });
    cy.fixture('signup/error_response').then((data) => {
      errorResponse = data as ErrorResponse;
    });
  });

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