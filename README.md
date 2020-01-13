Jaxon Library for Laravel
=========================

This package integrates the [Jaxon library](https://github.com/jaxon-php/jaxon-core) into the Laravel 5 framework.

Features
--------

- Automatically register Jaxon classes from a preset directory.
- Read Jaxon options from a file in Laravel config format.

Installation
------------

Add the following lines in the `composer.json` file, and run the `composer update` command.
```json
"require": {
    "jaxon-php/jaxon-laravel": "^3.1"
}
```

Add the following line to the `providers` entry in the `app.php` config file.
```php
Jaxon\Laravel\JaxonServiceProvider::class
```

Add the following line to the `aliases` entry in the `app.php` config file.
```php
'LaravelJaxon' => Jaxon\Laravel\Facades\Jaxon::class
```

Publish the package configuration.
```php
php artisan vendor:publish --tag=config
```

Edit the `config/jaxon.php` file to suit the needs of your application.

Configuration
------------

The settings in the jaxon.php config file are separated into two sections.
The options in the `lib` section are those of the Jaxon core library, while the options in the `app` sections are those of the Laravel application.

The following options can be defined in the `app` section of the config file.

| Name | Description |
|------|---------------|
| directories | An array of directory containing Jaxon application classes |
| views   | An array of directory containing Jaxon application views |
| | | |

By default, the `views` array is empty. Views are rendered from the framework default location.
There's a single entry in the `directories` array with the following values.

| Name | Default value | Description |
|------|---------------|-------------|
| directory | app_path('Jaxon/Classes') | The directory of the Jaxon classes |
| namespace | \Jaxon\App  | The namespace of the Jaxon classes |
| separator | .           | The separator in Jaxon class names |
| protected | empty array | Prevent Jaxon from exporting some methods |
| | | |

The `route` option is overriden by the `core.request.uri` option of the Jaxon library.

Usage
-----

This is an example of a Laravel controller using the Jaxon library.
```php
use Jaxon\Laravel\Jaxon;

class DemoController extends Controller
{
    public function __construct(Jaxon $jaxon)
    {
        $this->jaxon = $jaxon;
    }

    public function index()
    {
        // Print the page
        return view('index', [
            'JaxonCss' => $this->jaxon->css(),
            'JaxonJs' => $this->jaxon->js(),
            'JaxonScript' => $this->jaxon->script()
        ]);
    }
}
```

Before it prints the page, the controller calls the `$this->jaxon->css()`, `$this->jaxon->js()` and `$this->jaxon->script()` functions to get the CSS and javascript codes generated by Jaxon, which it inserts into the page.

### The Jaxon classes

The Jaxon classes can inherit from `\Jaxon\CallableClass`.
By default, they are located in the `app/Jaxon/Classes` dir of the Laravel application, and the associated namespace is `\Jaxon\App`.

This is a simple example of a Jaxon class, defined in the `app/Jaxon/Classes/HelloWorld.php` file.

```php
namespace Jaxon\App;

class HelloWorld extends \Jaxon\CallableClass
{
    public function sayHello()
    {
        $this->response->assign('div2', 'innerHTML', 'Hello World!');
        return $this->response;
    }
}
```

Contribute
----------

- Issue Tracker: github.com/jaxon-php/jaxon-laravel/issues
- Source Code: github.com/jaxon-php/jaxon-laravel

License
-------

The package is licensed under the BSD license.
