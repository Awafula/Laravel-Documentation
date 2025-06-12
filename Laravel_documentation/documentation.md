# Laravel Documentation

This is a Laravel documentation that captures whatever we have learnt at Dafric Logistics and Outside.

## what is Laravel and what does it do?

- Route Handling
- Security Layer
- Models and DB Migrations
- Views and Templates
- Handles Authentications and Sessions
- Compile Assets
- Storage and File Management
- Error handling
- Feature and Unit testing
- Email Configurations
- Cache Handling

## Artisan Command Line Interface

Laravel comes with the Artisan CLI that handles many tasks such as:

1. Creating Database migration files and running migrations
2. Session commands
3. Create custom commands
4. Create controllers and models
5. Show routes
6. Run Tinker
7. Create providers, events, jobs, form requests, etc.

## Installation and Environment Setup

## On Windows

1. Install Xampp or Wamp to set up Apache and Mysql Servers.
2. Open Xampp / Wamp Control panel and start Apache and Mysql
3. Install the latest version of PHP
4. Download and Install composer, which is a dependency management tool for PHP
5. Install laravel via the terminal, `composer global require laravel/installer`
6. Create a laravel project `laravel new project-name`
7. The command `composer create-project laravel/laravel appName` also creates a laravel application.
8. Run the project by navigating to the project directory via the terminal and run `php artisan serve` to start the laravel development server.

## On Linux

1. Install Xampp to set up Linux Apache and Mysql Servers.
2. Navigate to home/opt/lampp folder and start xampp. ie `sudo ./xampp start` to start Apache and Mysql
3. Install the latest version of PHP, ie. PHP 8.2
4. Download and Install composer, which is a dependency management tool for PHP  
   `curl -sS https://getcomposer.org/installer -o composer-setup.php`
5. To run the installer, do:  
   `sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer`
6. Verify composer installation  
   `composer -v`
7. Install laravel via the terminal,  
   `composer global require laravel/installer`
8. Create a laravel project  
   `laravel new project-name`
9. The command  
   `composer create-project laravel/laravel appName`  
   also creates a laravel application.
10. Run the project by navigating to the project directory via the terminal and run  
    `php artisan serve` to start the laravel development server.

## Database Migrations

Migrations are like version control for your db. They allow one to define and share the application's database schema definition. The Laravel Schema facade provides database agnostic support for creating and manipulating tables across all of Laravel's supported database systems. Typically, migrations will use this facade to create and modify database tables and columns.

## Before generating a Migration

1. Create a database using proper db naming conventions.
2. On the `.env` file, set up the correct environment settings, for example:
    - `DB_CONNECTION=mysql`
    - `DB_HOST=127.0.0.1`
    - `DB_PORT=3306`
    - `DB_DATABASE=studio`
    - `DB_USERNAME=root`
    - `DB_PASSWORD=`
3. To test your connection run `php artisan migrate`

## Generating Migrations

We use the `php artisan make:migration` artisan command to generate a database migration. The migration will be placed in the `database/migrations` directory. For example:

## Migration strucure

A migration class contains two methods: up and down. The up method is used to add new tables, columns, or indexes to your database, while the down method should reverse the operations performed by the up method.

This is how a migration structure looks like:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('flights', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('airline');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::drop('flights');
    }
};

```

## Running Migrations

To run all of your outstanding migrations, execute the migrate Artisan command:

`php artisan migrate`

To see how migrations one has run thus far,

`php artisan migrate:status`

## Drop all tables and Migrate

`php artisan migrate:fresh`

## How to Export a Database Using phpMyAdmin and Add it to a Laravel Project

---

## Part 1: Exporting Your Database from phpMyAdmin

### 🔹 Steps

1. Open [phpMyAdmin](http://localhost/phpmyadmin) in your browser.
2. Select the **database** you want to export from the left sidebar.
3. Click the **Export** tab at the top.
4. Choose the **export method**:
   - `Quick` – good for standard exports.
   - `Custom` – lets you choose tables and more options.
5. Set **Format** to `SQL`.
6. Click **Go** to download the `.sql` file.

---

## Part 2: Adding the `.sql` File to Your Laravel Project

You can optionally store the SQL file in your `\Laravel_project_directory\the_database_directory or [the folder containing Laravel backend files]`

## Github

- Commit the new changes made to the project directory `[that is adding the database directory]` to github repository

- The collaborators should now be able to pull the changes made and have an exact copy of the `.sql` file in their local machine

## Are you a collaborator?

- Do a  `git pull` to obtain the updated project files

- Create a `.env` file inside the backend directory and copy-paste the contents of `.env.example` into the new `.env` file

- Make proper changes to namings in the `DB_DATABASE= ...`

- ## Create a database in phpMyAdmin

1. Visit `http://localhost/phpmyadmin.`

2. Click New and create a new database (e.g., nyimbo_zetu).

- Make sure the name matches DB_DATABASE in your .env file.

- ## Import the  `.sql` File into the New Database

- After creating the database, click its name from the sidebar.

- Click the Import tab.

- Click Choose File and select the `.sql` file (e.g.,`database/sql/nyimbo_zetu.sql`).

- Click Go.

- You’ll see a success message if the import worked correctly.

- ## Confirm `.sql` File Configuration
  
- Make sure your `.env` file contains the correct values:

1. `DB_CONNECTION=mysql`
2. `DB_HOST=127.0.0.1`
3. `DB_PORT=3306`
4. `DB_DATABASE=nyimbo_zetu`
5. `DB_USERNAME=root`
6. `DB_PASSWORD=`

- ## Test the Database Connection

- Run the following command in your VS Code terminal or command line: ` php artisan migrate:status `

- If Laravel connects to your database, it will list migration statuses.

- If there’s a connection error, Laravel will throw a meaningful message

- ## 🛑 Important: Should You Run `php artisan migrate`?

## ❌ In most cases: No

- Since your `.sql` file already contains the tables and data, you don’t need to run: `php artisan migrate`

- Running this may overwrite or duplicate what’s already in the `.sql` file unless migrations were explicitly tracked.

- ## ✅ When SHOULD You Run Migrations?

- If you want to apply new migrations your teammate added.

- Check for new migration files in the `database/migrations` folder.

- If found, and they aren't yet applied, run: `php artisan migrate`

## `

## MODELS

## STEP 1: MAKING MIGRATIONS

1. Make migrations using ` php artisan make:migration create_users_table `: This is making a table .
2. Run the command `php artisan migrate` which runs the migrations thereby creating the tables

## ❌ N/B : Once you run it you can never rerun, if you add new columns use ` php artisan migrate: fresh ` which drops all the tables and the data and recreate the tables without the data

## STEP 2: CREATING MODELS

- Models are created manually using `php artisan make:model User.php` and naming a file as ` i.e. User.php `

- ### Rule of thumb

- The file name should be in singular [Not the way it is in the migration file naming]
- The file name should start with capital letter.

## STEP 3 : FACTORY CREATION

- Factory is used to create fake data for testing purposes

- - ### STEPS TO FOLLOW

1. In the console/terminal use ` php artisan tinker ` to allow you to interact with your laravel application from the command line.

2. Use the following commands to create "fake" entries

- ` App\Models\User::factory()->create(); `

- ` App\Models\User::factory(5)->create(); ` // to generate 5 entries

- ### Factory Naming Convention

- The naming of the factory should follow this convention >>> ` UserFactory.php `
- - Use uppercase letters at the beginning of each word.
- - The name should have a trailing extension `.php`
-
-
-

## Refer to Graphql documentation
