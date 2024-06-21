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

