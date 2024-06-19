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

### app.module.ts
##### Import `GraphQLModule` and `HttpClientModule` into app.module to make them global
```
...
import { GraphQLModule } from './graphql/graphql.module';
import { HttpClientModule } from '@angular/common/http'; 

@NgModule({
  ...
  imports: [..., GraphQLModule, HttpClientModule],
  ...
})
export class AppModule {}
```