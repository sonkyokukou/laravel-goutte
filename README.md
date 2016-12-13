# Laravel 5 Facade for Goutte

This repository implements a simple [ServiceProvider](https://laravel.com/docs/master/providers) that makes a singleton instance of the Goutte client easily accessible via a [Facade](https://laravel.com/docs/master/facades) in [Laravel 5](http://laravel.com). See [@FriendsOfPHP/Goutte](https://github.com/FriendsOfPHP/Goutte) for more information about the php web scraper and its interfaces.

## Installation using [Composer](https://getcomposer.org/)

In your terminal application move to the root directory of your laravel project using the `cd` command and require the project as a dependency using composer.

```sh
$ cd ~/Sites/laravel-example-project
$ composer require sonkyokukou/goutte:@dev
```

This will add the following lines to your `composer.json` and download the project and its dependencies to your projects directory:

```json
// ./composer.json


  "require": {
    "php": ">=5.5.9",
    "laravel/framework": "5.2.*",
    "sonkyokukou/goutte": "@dev",
    // ...
  },

```


## Usage

In ordet to use the static interface we first have to customize the application configuration to tell the system where it can find the new service. Open the file `config/app.php` in the editor of your choice and add the following lines (`[1]`, `[2]`):

```php
// config/app.php

return [

  // ...

  'providers' => [

    // ...

    /*
     * Application Service Providers...
     */
    App\Providers\AppServiceProvider::class,
    App\Providers\AuthServiceProvider::class,
    App\Providers\EventServiceProvider::class,
    App\Providers\RouteServiceProvider::class,
    Weidner\Goutte\GoutteServiceProvider::class, // [1]

  ],

  // ...

  'aliases' => [

    'App' => Illuminate\Support\Facades\App::class,
    'Artisan' => Illuminate\Support\Facades\Artisan::class,

    // ...

    'View' => Illuminate\Support\Facades\View::class,
    'Goutte' => Weidner\Goutte\GoutteFacade::class, // [2]
  ],

];

```

Now you should be able to use the facade within your application. Laravel will autoload the corresponding classes once you use the registered alias.

```php
// app/Http/routes.php

Route::get('/', function() {
  $crawler = Goutte::request('GET', 'http://duckduckgo.com/?q=Laravel');
  $url = $crawler->filter('.result__title > a')->first()->attr('href');
  dump($url);
  return view('welcome');
});
```

*TIP:* If you retrieve a "Class 'Goutte' not found"-Exception try to update the autoloader by running `composer dump-autoload` in your project root.
