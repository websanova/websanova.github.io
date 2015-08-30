---
layout: master
title: "Creating a Laravel 5.x Package :: Part 2 - Basic Development"
keywords: php, laravel, package, creating, development, controller, view, routes, service provider, service, provider, facade, websanova
description: In Part 2 we cover some basic package development. We cover, controllers, views and routes and get some working code up and running.
permalink: /blog/laravel/creating-a-new-package-in-laravel-5-part-2-controllers-routes-and-views.html
date: Aug 26 2015
---

* [Part 1 - Package Workflow](/blog/laravel/creating-a-new-package-in-laravel-5-part-1-package-workflow)
* [Part 2 - Controllers, Routes and Views](/blog/laravel/creating-a-new-package-in-laravel-5-part-2-controllers-routes-and-views)
* [Part 3 - Config and Asset Publishing](/blog/laravel/creating-a-new-package-in-laravel-5-part-3-config-and-asset-publishing)
* [Part 4 - Database, Migrations and Seeding](/blog/laravel/creating-a-new-package-in-laravel-5-part-4-database-migrations-and-seeding)
* [Part 5 - Unit Testing](/blog/laravel/creating-a-new-package-in-laravel-5-part-5-unit-testing)
* [Demo Code on GiHub](https://github.com/websanova/laravel-demo)

## Intro

For part 2 of the Laravel Package Creation series we will jump straight into development and get some working coding. We will cover `routes`, `controllers` and `views`.

## `src` Folder

You can pretty much create whatever structure you like within your package. However it's best practice to have a `src directory to keep all your code in along with the composer.json file at the root of the project.

~~~
.\pacakges\websanova\demo\src
.\pacakges\websanova\demo\composer.json
~~~

## composer.json

We will add a few additional fields to the `composer.json` file at the root of our Laravel package.

~~~
{
    "name": "websanova/demo",
    "description": "Demo package for Websanova article.",
    "keywords": ["laravel", "demo"],
    "license": "MIT",
    "authors": [
        {
            "name": "websanova",
            "email": "rob@websanova.com"
        }
    ],
    "autoload": {
        "psr-4": {
            "Websanova\\Demo\\": "src/"
        }
    }
    "require": {
        "illuminate/support": "~5"
    }
}
~~~

The `autoload` field tells us where to look when we refer to our packages namespace. This will be necessary so make sure you pick something unique for your package.

It really depends what kind of package we are developing, but for the sake of simplicity let's assume we are creating some service that will require a `ServiceProvider`. In this case we will need to require the `illuminate/support` library.

## Laravel

Next we will need to update each version of Laravel we want to test our package in. This will usually only include updating the `config/app.php` file with appropriate Facades and ServiceProviders. We will also need to update the composer.json file to tell it where to find our package.

**config/app.php**

~~~
'providers' => [
	...
	'Websanova\Demo\DemoServiceProvider',
	...
],

'aliases' => [
	...
	'Demo' => 'Websanova\Demo\DemoFacade',
	...
],
~~~

**composer.json**
~~~
...
"autoload": {
	...
	"psr-4": {
		"App\\": "app/",
		"Websanova\\Demo\\": "packages/Websanova/Demo/src/"
	}
},
...
~~~

## Package Class

Now that we are setup let's create a basic class, service provider and facade. Note that a service provider and facade are not necessary and we are only creating them to illustrate a basic example here.

**src/Demo.php**
~~~
namespace Websanova\Demo;

class Demo
{
    public function hello()
    {
        return 'hello';
    }
}
~~~

**src/DemoServiceProvider.php**
~~~
namespace Websanova\Demo;

use Illuminate\Support\ServiceProvider;

class DemoServiceProvider extends ServiceProvider
{
    public function register()
    {
        $this->app->bind('websanova-demo', function() {
            return new Demo;
        });
    }
}
~~~

**src/DemoFacade.php**
~~~
namespace Websanova\Demo;

use Illuminate\Support\Facades\Facade;

class DemoFacade extends Facade
{
    protected static function getFacadeAccessor() { 
        return 'websanova-demo';
    }
}
~~~

With the proper entries in the `config/app.php` file above we should now be able to call:

~~~
Demo::hello();
~~~

An important thing to note here is the `webanova-demo` name we are binding to. Make sure you pick something unique here (typically the same as your package name). This ensures we avoid collisions with other packages.

## Routes

In the above example we already have a basic working packing. If we are just building a simple facade accessible library then we would just work from there. However Sometimes we want to build some additional functionality that includes routes for the user ready to use.

We will need to add a `boot` method to our `DemoServiceProvider`. This `boot` method is where we will connect many things like routes and migrations later on.

**src/DemoServiceProvider.php**
~~~
namespace Websanova\Demo;

use Illuminate\Support\ServiceProvider;

class DemoServiceProvider extends ServiceProvider
{
    public function register()
    {
        $this->app->bind('websanova-demo', function() {
            return new Demo;
        });
    }

    public function boot()
    {
        require __DIR__ . '/Http/routes.php';
    }
}
~~~

We will then of course also need to create a `src/Http/routes.php` file. We can then create routes as we normally would in Laravel.

**src/Http/routes.php**
~~~
Route::get('demo/test', function () {
	return 'Test';
});

Route::get('demo/hello', function () {
	return Demo::hello();
});
~~~

Assuming our server is running we then navigate to `http://localhost:8000/demo/test` and `http://localhost:8000/demo/hello`.

## Controllers

If we have a lot of routes, we may want to organize our routes into controllers. Let's create a sample controller first.

**src/Http/DemoController.php**
~~~
namespace Websanova\Demo\Http;

use App\Http\Controllers\Controller;

class DemoController extends Controller
{
    public function index()
    {
        return \Demo::hello() . ' from controller.';
    }
}
~~~

We then add the following line to our `routes.php` file.

Route::get('demo', 'Websanova\Demo\Http\DemoController@index');

Navigate to `http://localhost:8000/demo` and we should see our text.

## Views

If we have a lot of complex views we also want to consider organizing our packages views. We do this by first creating an additional entry in our service providers `boot` method.

~~~
public function boot()
{
    require __DIR__ . '/Http/routes.php';

    $this->loadViewsFrom(__DIR__ . '/views', 'websanova-demo');
}
~~~

Here we specify the location to load our views from. In this case we chose to store them in `src/views` form the root of our project.

You will also see the second argument in the `loadViewsFrom` call. This is to create an alias when calling our views. Make sure to pick something unique here and preferably the same as your packages name to avoid collisions.

We can then create a sample view:

**src/views/index.php**
~~~
{{ Demo::hello() . ' from index view.' }}
~~~

And create a route to call our view:

~~~
Route::get('demo/view', function () {
	return view('websanova-demo::index');
});
~~~

## Conclusion

In this tutorial we covered quite a bit. But with a basic workflow and understanding of package development you will be well on your way to creating and publishing packages.