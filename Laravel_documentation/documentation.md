# Laravel Documentation

<p>This is a laravel documentation that captures whatever we have learnt at Dafric Logistics and Outside.</p>

# what is Laravel and what does it do?

<ul>
    <li>Route Handling</li>
    <li>Security Layer</li>
    <li>Models and DB Migrations</li>
    <li>Views and Templates </li>
    <li>Handles Authentications and Sessions</li>
    <li>Compile Assets</li>
    <li>Storage and File Management</li>
    <li>Error handling</li>
    <li>Feature and Unit testing</li>
    <li>Email Configurations</li>
    <li>Cache Handling</li>
</ul>

# Artisan Command Line Interface

<p>Laravel comes with the Artisan CLI that handles many tasks such as 
    <ol>
        <li>Creating Database migration files and running migrations</li>
        <li>Session commands</li>
        <li>Create custom commands</li>
        <li>Create controllers and models</li>
        <li>show routes</li>
        <li>Run Tinker</li>
        <li>Create providers, events, jobs, form requests, etc.</li>
    </ol>
</p>

# Installation and Environment Setup

## On Windows

<ol>
    <li>Install Xampp or Wamp to set up Apache and Mysql Servers.</li>
    <li>Open Xampp / Wamp Control panel and start Apache and Mysql</li>
    <li>Install the latest version of PHP</li>
    <li>Download and Install composer, which is a dependency management tool for PHP</li>
    <li>Install laravel via the terminal, <code>composer global require laravel/installer</code></li>
    <li>create a laravel project <code>laravel new project-name</code></li>
    <li>The command <code>composer create-project laravel/laravel appName</code> also creates a laravel application. </li>
    <li>Run the project , that is navigate to the project directory, via the terminal and run <code> php artisan serve </code> to start the laravel development server.</li>
</ol>

## On Linux

<ol>
    <li>Install Xampp to set up Linux Apache and Mysql Servers.</li>
    <li>navgate to home/opt/lampp folder and start xampp. ie <code>sudo ./xampp start</code> to start Apache and Mysql</li>
    <li>Install the latest version of PHP, ie. PHP 8.2</li>
    <li>Download and Install composer, which is a dependency management tool for PHP<code>curl -sS https://getcomposer.org/installer -o composer-setup.php
    </code></li>
    <li>To run the installer, do:<code>sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer</code>
    <li>Verify composer installation<code>composer -v</code>
    <li>Install laravel via the terminal, <code>composer global require laravel/installer</code></li>
    <li>create a laravel project <code>laravel new project-name</code></li>
    <li>The command <code>composer create-project laravel/laravel appName</code> also creates a laravel application. </li>
    <li>Run the project , that is navigate to the project directory, via the terminal and run <code> php artisan serve </code> to start the laravel development server.</li>
</ol>

# Database Migrations

<p>Migrations are like version control for your db. They allow one to define and share the application's database schema definition. The Laravel Schema facade provides database agnostic support for creating and manipulating tables across all of Laravel's supported database systems. Typically, migrations will use this facade to create and modify database tables and columns.</p>

## Before generating a Migration

<ol>
    <p>create a database using proper db naming conventions.</p>
    <p>On the .env file, setup the correct environment settings, for example: 
    <li>DB_CONNECTION=mysql</li>
     <li>DB_HOST=127.0.0.1</li>
     <li>DB_PORT=3306</li>
     <li>DB_DATABASE=studio</li>
     <li>DB_USERNAME=root</li>
     <li>DB_PASSWORD=</li>
    </p>
</ol>
<P>to test your connection run <code>php artisan migrate</code></p>

## Generating Migrations

<p>We use the <mark>php artisan make:migration</mark> artisan command to generate a database migration. The migration willl be placed in the <i><mark>databse/migrations,</mark></i> directory. eg. </p>

<code> php artisan make:migration cerate_users_table</code>

## Migration strucure

<p>A migration class contains two methods: up and down. The up method is used to add new tables, columns, or indexes to your database, while the down method should reverse the operations performed by the up method.</p>

<p>This is how a migration structure looks like:</p>

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

<p>To run all of your outstanding migrations, execute the migrate Artisan command:</p>

<code>php artisan migrate </code>

<p>To see how migrations one has run thus far, </p>

<code> php artisan migrate:status </code>

## Drop all tables and Migrate

<code> php artisan migrate:fresh </code>



# How to Export a Database Using phpMyAdmin and Add it to a Laravel Project

---

##  Part 1: Exporting Your Database from phpMyAdmin

### 🔹 Steps:

1. Open [phpMyAdmin](http://localhost/phpmyadmin) in your browser.
2. Select the **database** you want to export from the left sidebar.
3. Click the **Export** tab at the top.
4. Choose the **export method**:
   - `Quick` – good for standard exports.
   - `Custom` – lets you choose tables and more options.
5. Set **Format** to `SQL`.
6. Click **Go** to download the `.sql` file.

---

##  Part 2: Adding the `.sql` File to Your Laravel Project

You can optionally store the SQL file in your <code> \Laravel_project_directory\the_database_directory or [the folder containing Laravel backend files]</code>







# Github

- Commit the new changes made to the project directory <code> [that is adding the database directory]</code> to github repository

- The collaborators should now be able to pull the changes made and have an exact copy of the <code> .sql </code>  file in their local machine

# Are you a collaborator?

- Do a <code> git pull </code> to obtain the updated project files

- Create a  <code> .env </code> file inside the backend directory and copy-paste the contents of <code> .env.example </code> into the new <code>.env</code> file

- Make proper changes to namings in the <code> DB_DATABASE= ... </code> 

- ## Create a database in phpMyAdmin

 1. Visit http://localhost/phpmyadmin.

 2. Click New and create a new database (e.g., nyimbo_zetu).

   - Make sure the name matches DB_DATABASE in your .env file.

-  ## Import the <code> .sql </code> File into the New Database

- After creating the database, click its name from the sidebar.

- Click the Import tab.

- Click Choose File and select the <code>.sql</code> file (e.g.,<code> database/sql/nyimbo_zetu.sql</code>).

- Click Go.

- You’ll see a success message if the import worked correctly.

- ## Confirm <code>.sql</code>  File Configuration 
- Make sure your <code> .env </code> file contains the correct values:

<ol>     
    <li>DB_CONNECTION=mysql</li>
     <li>DB_HOST=127.0.0.1</li>
     <li>DB_PORT=3306</li>
     <li>DB_DATABASE=nyimbo_zetu</li>
     <li>DB_USERNAME=root</li>
     <li>DB_PASSWORD=</li>
    
</ol>

- ## Test the Database Connection

- Run the following command in your VS Code terminal or command line: <code> php artisan migrate:status </code>

- If Laravel connects to your database, it will list migration statuses.

- If there’s a connection error, Laravel will throw a meaningful message

- ## 🛑 Important: Should You Run <code>php artisan migrate</code>?

## ❌ In most cases: No!
- Since your <code>.sql</code> file already contains the tables and data, you don’t need to run: <code>php artisan migrate</code>

- Running this may overwrite or duplicate what’s already in the <code>.sql</code> file unless migrations were explicitly tracked.


- ## ✅ When SHOULD You Run Migrations?

- If you want to apply new migrations your teammate added.

- Check for new migration files in the <code>database/migrations</code> folder.

- If found, and they aren't yet applied, run: <code>php artisan migrate</code>
  

  
#
#
#
#

# MODELS

## STEP 1: MAKING MIGRATIONS

 1. Make migrations using <code> php artisan make:migration create_users_table </code>
- This is making a table
  2. Run the command <code>php artisan migrate </code> which runs the migrations thereby creating the tables
   ## ❌ N/B : Once you run it you can never rerun, if you add new columns use <code> php artisan migrate: fresh </code> which drops all the tables and the data and recreate the tables without the data 


## STEP 2: CREATING MODELS

- Models are created manually by creating and naming a file as <code> i.e. User.php </code>
- ### Rule of thumb

- The file name should be in singular [Not the way it is in the migration file naming]
- The file name should start with capital letter.  

## STEP 3 : FACTORY CREATION

- Factory is used to create fake data for testing purposes
- - ### STEPS TO FOLLOW

1. In the console/terminal use <code> php artisan tinker </code> to allow you to interact with your laravel application from the command line.

2. Use the following commands to create "fake" entries

- <code>  App\Models\User::factory()->create(); </code>

- <code>  App\Models\User::factory(5)->create(); </code> // to generate 5 entries 


- ### Rule of thumb
- The naming of the factory should follow this convention >>> <code> UserFactory.php </code> 
- - 1. Use uppercase letters at the beginning of each word.
-  - 2. The name should have a trailing extension <code> .php </code>




