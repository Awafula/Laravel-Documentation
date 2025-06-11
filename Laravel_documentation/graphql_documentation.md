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
