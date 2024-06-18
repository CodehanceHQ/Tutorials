### GraphQL
```
GraphQL is a query language for APIs and a runtime for executing those queries by using a type system you define for your data.

It allows clients to request exactly the data they need, making it more efficient and flexible than traditional REST APIs.
```

### Apollo
```
Apollo is a comprehensive state management library for JavaScript that enables you to manage both local and remote data with GraphQL.

It helps in connecting your GraphQL server to your client application, providing tools to make GraphQL queries and mutations, and handling state management.
```

### Mutations
```
Mutations are used in GraphQL to modify server-side data. They are similar to the POST, PUT, DELETE methods in REST APIs.

A mutation might be used to create, update, or delete data. Each mutation can optionally return data, making it easy to get the new state of data after the mutation.
```

### Queries
```
Queries are used in GraphQL to fetch data from the server. They are similar to the GET method in REST APIs.

A query allows the client to specify exactly what data they need, and the server responds with only that data, reducing the amount of data transferred over the network.
```

### folder sturcure
```
graphql/
├── graphql.module.ts  # This is the main module file where GraphQL configurations are set up.
├── mutations/
│   ├── signUp.ts       # This file contains a GraphQL mutation for user sign-up.
│   └── anotherMutation.ts  # This file contains another GraphQL mutation for different operations.
└── queries/
    ├── getUser.ts      # This file contains a GraphQL query to get user data.
    └── anotherQuery.ts # This file contains another GraphQL query for different operations.
```

### Install apollo graphql client
```
npm install apollo-angular @apollo/client graphql

# questions:
# 1: What is graphql
# 2: What is apollo?
# 3: how does graphql differ from REST?
```

### create structure
```
mkdir src/app/graphql
touch src/app/graphql/graphql.module.ts
mkdir src/app/graphql/mutations
mkdir src/app/graphql/queries
```

### edit graphql.module.ts
```
import { NgModule } from '@angular/core';
import { ApolloModule, APOLLO_OPTIONS } from 'apollo-angular';
import { HttpLink } from 'apollo-angular/http';
import { InMemoryCache, ApolloClientOptions } from '@apollo/client/core';

const uri = 'http://127.0.0.1:3000/graphql'; // Your GraphQL endpoint

export function createApollo(httpLink: HttpLink): ApolloClientOptions<any> {
  return {
    link: httpLink.create({ uri }),
    cache: new InMemoryCache(),
  };
}

@NgModule({
  imports: [ApolloModule],
  providers: [
    {
      provide: APOLLO_OPTIONS,
      useFactory: createApollo,
      deps: [HttpLink],
    },
  ],
})
export class GraphQLModule {}
```

### graphql.module.spec.ts
```
touch src/app/graphql/graphql.module.spec.ts
npm install -g @angular/cli

import { TestBed } from '@angular/core/testing';
import { ApolloClient, InMemoryCache } from '@apollo/client/core';
import { APOLLO_OPTIONS, ApolloModule } from 'apollo-angular';
import { HttpClientModule } from '@angular/common/http';
import { GraphQLModule, createApollo } from './graphql.module';
import { HttpLink } from 'apollo-angular/http';

describe('GraphQLModule', () => {
  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [
        HttpClientModule,
        ApolloModule,
        GraphQLModule
      ],
      providers: [
        {
          provide: APOLLO_OPTIONS,
          useFactory: createApollo,
          deps: [HttpLink]
        }
      ]
    }).compileComponents();
  });

  it('should create the GraphQL module', () => {
    const module = TestBed.inject(GraphQLModule);
    expect(module).toBeTruthy();
  });

  it('should provide Apollo Client', () => {
    const apolloOptions = TestBed.inject(APOLLO_OPTIONS);
    expect(apolloOptions).toBeTruthy();
    expect(apolloOptions.link).toBeDefined();
    expect(apolloOptions.cache).toBeInstanceOf(InMemoryCache);
});

it('should configure Apollo Client with authLink and httpLink', () => {
    const apolloOptions = TestBed.inject(APOLLO_OPTIONS);
    const httpLink = TestBed.inject(HttpLink);

    expect(apolloOptions).toBeTruthy();
    expect(apolloOptions.link).toBeDefined();
    expect(apolloOptions.link?.request).toBeDefined();

    const apolloClient = new ApolloClient({
      link: apolloOptions.link!,
      cache: apolloOptions.cache!,
    });

    expect(apolloClient.link).toBeDefined();
    expect(apolloClient.cache).toBeInstanceOf(InMemoryCache);
});
});

```

