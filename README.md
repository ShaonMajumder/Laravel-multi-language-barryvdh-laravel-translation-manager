<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## Pros
- You do not have to replace any internal class
- you can access translation in laravel way
- WebInterface is user friendly
- Supports wide range of laravel and php versions comparitively

## Cons
- Have to backup everytime before publishing or writing to langauge file; 
- handle `Lang` directory with caution

## Description
- Does not replace any internal file, you can access translation in laravel way
- It does not replace rather it can replace existing lang file.
- Can manage and insert new translation through webinterface and save them in db.
- Later can export in lang file, it replaces existing lang file.

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

### custom functionality
for custom function override
```bash
composer dump-autoload
```

### NB
Routes are added in the ServiceProvider. You can set the group parameters for the routes in the configuration. You can change the prefix or filter/middleware for the routes. If you want full customisation, you can extend the ServiceProvider and override the map() function.

This example will make the translation manager available at http://yourdomain.com/translations

### Middleware / Auth
The configuration file by default only includes the auth middleware, but the latests changes in Laravel 5.2 makes it that session variables are only accessible when your route includes the web middleware. In order to make this package work on Laravel 5.2, you will have to change the route/middleware setting from the default<br>
config/translation-manager.php :
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

### To set hardcoded language
AppServiceProvider.php edit
```php
/**
 * Bootstrap any application services.
 *
 * @return void
 */
public function boot()
{
    app()->setLocale('bn');
}
```
## Usage
### Precaution
- Always backup your database before importing
- Always backup your lang directories and files before exporting

### Adding New Language
Insert into 'Enter new locale key:'
Then press 'Add new locale'

### Find Translation
Press button 'Find Translation in Files' to import translations from file

### Enter new group or page name
Insert into text 'Enter a new group name and start edit translations in that group'
Press button 'Add and edit keys'

### Add new keys to this group
Insert into text 'Add new keys to this group'
Press button 'Add Keys'

### Edit Languages
After adding keys empty field can be added or deleted in list shown below.


### Web interface Summary

When you have imported your translation (via buttons or command), you can view them in the webinterface (on the url you defined with the controller).
You can click on a translation and an edit field will popup. Just click save and it is saved :)
When a translation is not yet created in a different locale, you can also just edit it to create it.

Using the buttons on the webinterface, you can import/export the translations. For publishing translations, make sure your application can write to the language directory.

You can also use the commands below.

### Import command

The import command will search through app/lang and load all strings in the database, so you can easily manage them.

```
php artisan translations:import
```

Translation strings from app/lang/locale.json files will be imported to the __json_ group.
    
Note: By default, only new strings are added. Translations already in the DB are kept the same. If you want to replace all values with the ones from the files, 
add the `--replace` (or `-R`) option: `php artisan translations:import --replace`

### Find translations in source

The Find command/button will look search for all php/twig files in the app directory, to see if they contain translation functions, and will try to extract the group/item names.
The found keys will be added to the database, so they can be easily translated.
This can be done through the webinterface, or via an Artisan command.

```
php artisan translations:find
```
    
If your project uses translation strings as keys, these will be stored into then __json_ group. 

### Export command

The export command will write the contents of the database back to app/lang php files.
This will overwrite existing translations and remove all comments, so make sure to backup your data before using.
Supply the group name to define which groups you want to publish.

```
php artisan translations:export <group>
```

For example, `php artisan translations:export reminders` when you have 2 locales (en/nl), will write to `app/lang/en/reminders.php` and `app/lang/nl/reminders.php`

To export translation strings as keys to JSON files , use the `--json` (or `-J`) option: `php artisan translations:export --json`. This will import every entries from the __json_ group.

### Clean command

The clean command will search for all translation that are NULL and delete them, so your interface is a bit cleaner. Note: empty translations are never exported.

```
php artisan translations:clean
```

### Reset command

The reset command simply clears all translation in the database, so you can start fresh (by a new import). Make sure to export your work if needed before doing this.

```
php artisan translations:reset
```


### Detect missing translations

Most translations can be found by using the Find command (see above), but in case you have dynamic keys (variables/automatic forms etc), it can be helpful to 'listen' to the missing translations.
To detect missing translations, we can swap the Laravel TranslationServiceProvider with a custom provider.
In your `config/app.php`, comment out the original TranslationServiceProvider and add the one from this package:

    //'Illuminate\Translation\TranslationServiceProvider',
    'Barryvdh\TranslationManager\TranslationServiceProvider',

This will extend the Translator and will create a new database entry, whenever a key is not found, so you have to visit the pages that use them.
This way it shows up in the webinterface and can be edited and later exported.
You shouldn't use this in production, just in development to translate your views, then just switch back.

## TODO

This package is still very alpha. Few things that are on the todo-list:

    - Add locales/groups via webinterface
    - Improve webinterface (more selection/filtering, behavior of popup after save etc)
    - Seed existing languages (https://github.com/caouecs/Laravel-lang)