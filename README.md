<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## Installation
### set up laravel
```bash
composer create-project laravel/laravel Laravel-multi-language-barryvdh-laravel-translation-manager
cd Laravel-multi-language-barryvdh-laravel-translation-manager
```
### setup joedixon/laravel-translation
```bash
composer require barryvdh/laravel-translation-manager
```
### Set env
cp .env.example .env
code .env
### Migration
You need to run the migrations for this package.
```bash
php artisan vendor:publish --provider="Barryvdh\TranslationManager\ManagerServiceProvider" --tag=migrations
php artisan migrate
```
### Publish
You need to publish the config file for this package. This will add the file config/translation-manager.php, where you can configure this package.
```bash
php artisan vendor:publish --provider="Barryvdh\TranslationManager\ManagerServiceProvider" --tag=config
```
### Edit
In order to edit the default template, the views must be published as well. The views will then be placed in resources/views/vendor/translation-manager.
```bash
php artisan vendor:publish --provider="Barryvdh\TranslationManager\ManagerServiceProvider" --tag=views
```

### NB
Routes are added in the ServiceProvider. You can set the group parameters for the routes in the configuration. You can change the prefix or filter/middleware for the routes. If you want full customisation, you can extend the ServiceProvider and override the map() function.

This example will make the translation manager available at http://yourdomain.com/translations

### Middleware / Auth
The configuration file by default only includes the auth middleware, but the latests changes in Laravel 5.2 makes it that session variables are only accessible when your route includes the web middleware. In order to make this package work on Laravel 5.2, you will have to change the route/middleware setting from the default
```php
    'route' => [
        'prefix' => 'translations',
        'middleware' => 'auth',
    ],
```
to
```php
    'route' => [
        'prefix' => 'translations',
        'middleware' => [
	        'web',
	        'auth',
		],
    ],
```
<b>NOTE:</b> This is only needed in Laravel 5.2 (and up!)

### Generate key
```php
php artisan key:generate
```
### Setting UI
```bash
composer require laravel/ui
php artisan ui vue
php artisan ui vue --auth
npm install
npm run watch
```
### Serve
```bash
php artisan serve
```

### Register user and navigate
- register user
- login
- navigate to http://127.0.0.1:8000/translations