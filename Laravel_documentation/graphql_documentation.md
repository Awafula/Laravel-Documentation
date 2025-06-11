# GraphQL Documentation

## 1. GraphQl installation

### Install lighthouse

- Use composer to install Lighthouse: Use this command in the terminal:

  ```bash
  composer require nuwave/lighthouse
  ```

- Publish the default schema to `graphql/schema.graphql`:

```bash
php artisan vendor:publish --tag=lighthouse-schema
```

- A graphql folder will be created with a `schema.graphql` file.

- Delete the content of the file and paste in this line

 ```bash
  #import */*.graphql

  ```

### Creating Types

- Create a folder named `Types` inside the `graphql` folder

- GraphQL types are the fundamental building blocks of a GraphQL schema, defining the structure and shape of the data that can be queried or manipulated through the API. They describe what data can be requested, how it is organized, and what operations are possible, thus forming a contract between client and server

- Syntax for creating type :

```bash
type User{
    id: ID!
    email: String!
    first_name: String!
    last_name: String!
    phone_number: String!
    address: String!
    email_verified_at: String!
    password: String!
    created_at: String!
    updated_at: String!
}

input CreateUserInput{
    id: ID!
    email: String!
    first_name: String!
    last_name: String!
    phone_number: String!
    address: String!
    password: String!
}

type CreateUserMutationResponse{
    message: String!
    user: User!
}
```

### Creating Queries

- Create another folder named `Queries` inside the `graphql` folder

- This kind of query is used to efficiently fetch a subset of `users` from potentially large datasets, enabling clients to request data page by page rather than retrieving all users at once. Pagination improves performance and user experience by reducing the amount of data transferred and processed in a single request.

- Syntax for these type of query is:

```bash
type Query {
  user: [User!]! @paginate(defaultCount: 10)
}
```

- The `@paginate` directive typically adds pagination parameters (like page or first, after) to the query, allowing clients to specify which page or slice of data they want to fetch. For example, clients can request the first 10 users, then the next 10, and so on.

### Creating Mutations

- Mutations in GraphQL are operations designed to modify server-side data, such as creating, updating, or deleting records. Unlike queries, which are read-only and fetch data, mutations allow clients to perform write operations that change the state of data on the server.

- Create an additional folder named `Mutations` inside the `graphql` folder

- Syntax for the mutation

```bash
extend type Mutation {
    createUser(input: CreateUserInput! @spread): CreateUserMutationResponse!
        @field(
            resolver: "App\\Graphql\\Mutations\\CreateUserMutation@createUser"
        )
}
```

## N/B: Only use the preceeding `extend` iff it is not the first mutation you have created else just start with `type ...`

- When a client calls the `createUser` mutation with a structured input `(CreateUserInput)`, the GraphQL server invokes the specified resolver method `createUser` in the `CreateUserMutation` class. The resolver receives the unpacked input fields (enabled by the`@spread` directive), performs the necessary business logic to create a new `user` in the database or data source, including validation and handling any side effects. Upon completion, the resolver returns a `CreateUserMutationResponse` object containing the newly created user data along with any relevant metadata or errors, which is then sent back to the client as the mutation result. In essence, the resolver acts as the server-side function that executes the core creation logic, effectively connecting the GraphQL schema to the underlying application or database operations.

### It is important to note that mutations cannot work without queries and each must have a resolver

### Creating resolvers

- To create a resolver for a mutaion, first you need to create a folder inside `app\` folder and name it `Graphql`

- Create another folder inside the `Graphql` folder and name it `Mutations`

- Inside the `Mutations` folder is where you now create your resolvers and they should be in a format similar to:

```bash
<?php

namespace App\Graphql\Mutations;

use App\Models\User;
use GraphQL\GraphQL;
use GraphQL\Type\Definition\ResolveInfo;
use Illuminate\Support\Facades\Hash;        
use Nuwave\Lighthouse\Support\Contracts\GraphQLContext;
use Nuwave\Lighthouse\Support\Contracts\GraphQLMutation;


class CreateUserMutation{
    public function createUser($root, array $args, GraphQLContext $context, ResolveInfo $resolveInfo)

    {
        $user=User::create([
            'email' => $args['email'],
            'first_name' => $args['first_name'],
            'last_name' => $args['last_name'],
            'phone_number' => $args['phone_number'],
            'address' => $args['address'],
            'password' => Hash::make($args['password']),
        ]);
        return [
            'user' => $user,
            'message' => 'User created successfully',
        ];
    
       
    }
}
```

- How the resolver works

1. Invocation: When the `createUser` mutation is called by a client, the GraphQL server invokes this resolver method, passing in the arguments `($args)` extracted from the mutation input.
2. User Creation: The resolver uses the `User::create()` method (an Eloquent ORM function in Laravel) to insert a new user record into the database. It maps the input fields `(email, first_name, last_name, phone_number, address)` directly from the `$args array`.
3. Password Hashing: Before storing the password, the resolver hashes it using Laravel’s `Hash::make()` function to ensure that raw passwords are never saved in the database, enhancing security.
4. Response Construction: After successfully creating the user, the resolver returns an array containing the newly created user object and a success `message`.
5. Result Delivery: This response is then sent back through GraphQL to the client, providing confirmation and the details of the created user.

## Testing the API

- After a successfully creating the api it is important to test whether it is working or not using either `postman` or `Altair` - an extension preferrably for microsoft edge browser

### using Altair extension

1. Run the command ` php artisan serve` to run the server locally
2. The server will be running on `http://127.0.0.1:8000/`
3. Inside the Altair window paste the url and add a trailing `/graphql/` so that the complete url is `http://127.0.0.1:8000/graphql`
4. Write a query to test your api following this syntax:

```bash
mutation {
  createUser(
    input: {
      id: 6
      email: "joyce.kanini@example.org"
      first_name: "Joyce"
      last_name: "Kanini"
      phone_number: "+254701234567"
      address: "89 Westlands Avenue, Nairobi"
      password: "Kanini@789"
    }
  ) {
    message
    user {
      id
      email
      first_name
      last_name
      phone_number
      address
      password
    }
  }
}
```

-This will createUser if there are no errors thrown and if there are errors, troubleshoot and ensure the api is working correctly before any further development

- For retrieving data from the database, make this type of query :

```bash
query users{
  user{
    data{
      email
      first_name
      last_name
      
    }
    paginatorInfo{
      currentPage
      lastPage
      total
    }
  }
}
```

- This query returns a paginated list of users, where each user object includes their email address, first name, and last name. Along with the user data, it provides pagination metadata such as the current page number, the total number of pages available, and the overall total count of users, enabling efficient navigation through large sets of user records.









## Form Validation in GraphQL and Laravel

<p>The simplest way to leverage the built-in validation rules is to use the <code>@rules</code> directive </p>

```
type Mutation {
    createUser(email: String @rules(apply: ["email"])): User
}

```

# Adding Rules

<code> @rules(apply:["required", "exists:outstaion, id"])</code>

<p> The required parameter insinuates that data for the field has to be provided. </p>
<p> unique - is a rule that says the field should be the only one with that particular kind of data </p>
<p> exists - additional rule that says the field must be there. </p>
<p> oustation here stands for the table name </p>
<p> id is the column name </p>

# Process for Adding Filters (e.g., `first` and `search`)

1. **Install Passport via composer.**

   ```bash
   composer require laravel/passport
   ```

   Laravel Passport provides a full OAuth2 server implementation for your Laravel application in a matter of minutes.

   Install Laravel Passport using the `install:api` Artisan command:

   ```bash
   php artisan install:api --passport
   ```

   This command will publish and run the database migrations necessary for creating the tables required to store OAuth2 clients and access tokens. Additionally, it will create the encryption keys needed for secure access tokens.

   You’ll also be prompted to use UUIDs as the primary key value for the Passport Client model instead of auto-incrementing integers.

2. **Set up the `apollo.config.js` file** in your frontend project to configure Apollo with the GraphQL API:

   ```javascript
   // apollo.config.js
   module.exports = {
     client: {
       service: {
         name: "blogs", // This is the name of your application.
         // URL to the GraphQL API
         url: "http://localhost:8000/graphql",
       },
       // Files processed by the extension
       includes: ["src/**/*.vue", "src/**/*.js"],
     },
   };
   ```

3. **Set up `apollo.default.config.js`** with the following code to establish a connection to the GraphQL API:

   ```javascript
   import {
     ApolloClient,
     createHttpLink,
     InMemoryCache,
   } from "@apollo/client/core";

   // HTTP connection to the API
   const httpLink = createHttpLink({
     // Use an absolute URL
     url: "http://localhost:8000/graphql",
   });

   // Cache implementation
   const cache = new InMemoryCache();

   // Create the Apollo client
   const apolloClient = new ApolloClient({
     link: httpLink,
     cache,
   });

   export { apolloClient };
   ```

4. **Add Filtering (eg. search and first parameters)**
   <li>In your vue component, set up a useQuery to execute the AUTHORS_QUERY with parameters like search and first passed as variables. This will allow the query to dynamically reflect filter changes. </li>
   <li> Thw AUTHORS_QUERY should look like this.

   ```javascript
   import gql from "graphql-tag";

   const AUTHORS_QUERY = gql`
     query authors($search: String, $page: Int, $first: Int) {
       authors(page: $page, first: $first, authorsSearch: { search: $search }) {
         data {
           first_name
           last_name
           rating
           user {
             email
             phone_number
           }
         }
       }
     }
   `;

   export { AUTHORS_QUERY };
   ```
