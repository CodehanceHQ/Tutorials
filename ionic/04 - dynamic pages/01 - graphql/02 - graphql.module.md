### Install apollo graphql client and storage
```
npm install apollo-angular @apollo/client graphql

# questions:
# 1: What is graphql
# 2: What is apollo?
# 3: how does graphql differ from REST?
```

### Install storage capability
```
npm install apollo-link-context
npm install @ionic/storage-angular
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
import { setContext } from '@apollo/client/link/context';
import { Storage } from '@ionic/storage-angular';

const uri = 'http://127.0.0.1:3000/graphql'; // Your GraphQL endpoint

export function createApollo(httpLink: HttpLink, storage: Storage): ApolloClientOptions<any> {
  // Define the context link to set the Authorization header
  const authLink = setContext(async (_, { headers }) => {
    // Retrieve the token from storage
    const token = await storage.get('authToken');
    return {
      headers: {
        ...headers,
        // Set the Authorization header if the token is available, otherwise set it to an empty string
        Authorization: token ? `Bearer ${token}` : '',
      },
    };
  });

  // Combine the authLink with the httpLink
  return {
    link: authLink.concat(httpLink.create({ uri })),
    cache: new InMemoryCache(),
  };
}

@NgModule({
  imports: [ApolloModule],
  providers: [
    {
      provide: APOLLO_OPTIONS,
      useFactory: createApollo,
      deps: [HttpLink, Storage],
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
import { IonicStorageModule, Storage } from '@ionic/storage-angular';

@NgModule({
  ...
  imports: [..., GraphQLModule, HttpClientModule, IonicStorageModule.forRoot()],
  ...
})

export class AppModule {
  constructor(private storage: Storage) {
    this.init();
  }

  async init() {
    await this.storage.create();
  }
}
```