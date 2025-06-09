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

## Before genrating a Migration

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

<code> php artisan migrate:fresh
