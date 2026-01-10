# GraphQL

## Basic Components of GraphQL

# Lighthouse Tutorial Summary

## Overview
- Introductory guide to building a **GraphQL server** with **Laravel** using Lighthouse.
- Beginner-friendly, but assumes some familiarity with GraphQL and Laravel.
- Source code available at [nuwave/lighthouse-tutorial](https://github.com/nuwave/lighthouse-tutorial).

Basic installation:
```bash
composer require nuwave/lighthouse
php artisan vendor:publish --tag=lighthouse-config
php artisan vendor:publish --tag=lighthouse-schema
```

This creates:
```
graphql/schema.graphql
```

## What is GraphQL?
- Query language for APIs and runtime for fulfilling queries.
- Provides:
  - Precise data fetching
  - Easier API evolution
  - Strong developer tools
- Uses **Schema Definition Language (SDL)** to define types and relationships.
- Example: `User` and `Post` types with one-to-many relationships.

Example SDL:
```graphql
type User {
  id: ID!
  name: String!
  posts: [Post!]! @hasMany
}

type Post {
  id: ID!
  title: String!
  body: String!
  user: User! @belongsTo
}
```

Example query:
```graphql
query {
  posts {
    id
    title
    user {
      name
    }
  }
}
```

## What is Lighthouse?
- A Laravel package to serve GraphQL APIs.
- Steps:
  1. Define data shape with SDL.
  2. Use directives to implement schema behavior.
  3. Add custom functionality as needed.

Common directives:
```graphql
@all
@find
@hasMany
@belongsTo
@create
@paginate
```

## Agenda
- Build a simple **Blog API** using:
  - Laravel
  - Lighthouse
  - GraphiQL

## Installation
- Start with a fresh Laravel project.
```bash
laravel new blog-api
cd blog-api
```

- Configure database and run migrations.
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=blog
DB_USERNAME=root
DB_PASSWORD=
```

```bash
php artisan migrate
```

- Test queries using GraphiQL:
```
http://localhost/graphql-playground
```

## Models
- **User**: can publish multiple posts.
- **Post**: belongs to a user, has many comments.
- **Comment**: belongs to a post.
- Relationships defined with Eloquent models and migrations.

User model:
```php
class User extends Authenticatable
{
    public function posts()
    {
        return $this->hasMany(Post::class);
    }
}
```

Post model:
```php
class Post extends Model
{
    protected $fillable = ['title', 'body', 'user_id'];

    public function user()
    {
        return $this->belongsTo(User::class);
    }

    public function comments()
    {
        return $this->hasMany(Comment::class);
    }
}
```

Comment model:
```php
class Comment extends Model
{
    protected $fillable = ['body', 'post_id'];

    public function post()
    {
        return $this->belongsTo(Post::class);
    }
}
```

## Schema
- GraphQL schema mirrors Eloquent models.
- Queries:
  - `posts`: fetch all posts.
  - `post(id: Int!)`: fetch a single post by ID.
- Types:
  - `User`, `Post`, `Comment` with `@hasMany` and `@belongsTo` directives.

```graphql
type Query {
  posts: [Post!]! @all
  post(id: ID! @eq): Post @find
}

type User {
  id: ID!
  name: String!
  posts: [Post!]! @hasMany
}

type Post {
  id: ID!
  title: String!
  body: String!
  user: User! @belongsTo
  comments: [Comment!]! @hasMany
}

type Comment {
  id: ID!
  body: String!
  post: Post! @belongsTo
}
```

Mutation example:
```graphql
type Mutation {
  createPost(
    title: String! @rules(apply: ["required", "min:3"])
    body: String! @rules(apply: ["required"])
  ): Post @create
}
```

## Result
- Seed database with sample data.
```php
Post::factory()
    ->count(10)
    ->for(User::factory())
    ->has(Comment::factory()->count(3))
    ->create();
```

- Query posts with authors and comments via GraphiQL.
```graphql
query {
  posts {
    id
    title
    user {
      name
    }
    comments {
      body
    }
  }
}
```

- Demonstrates GraphQL’s power in serving structured API data.

## Next Steps
- Add pagination.
```graphql
type Query {
  posts: [Post!]! @paginate
}
```

- Create/update models using mutations.
- Validate inputs using `@rules`.
- Extend features for deeper learning such as authentication, authorization, and subscriptions.

**Citation:**  
[Lighthouse Tutorial](https://lighthouse-php.com/tutorial)



