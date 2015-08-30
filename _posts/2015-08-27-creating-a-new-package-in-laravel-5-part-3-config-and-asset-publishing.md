---
layout: master
title: "Creating a Laravel 5.x Package :: Part 3 - Config and Asset Publishing"
keywords: php, laravel, package, creating, assets, publishing, config, configuration, websanova
description: In Part 3 we will build on what we learned in part 2. We will cover configuration files and publishing our package so that views and configs can be overwritten within an app.
permalink: /laravel/creating-a-new-package-in-laravel-5-part-3-config-and-asset-publishing.html
date: Aug 27 2015
---

* [Creating a Laravel 5.x Package :: Part 1 - Package Workflow](/creating-a-new-package-in-laravel-5-part-1-package-workflow)
* [Creating a Laravel 5.x Package :: Part 2 - Controllers, Routes and Views](/creating-a-new-package-in-laravel-5-part-2-controllers-routes-and-views)
* [Creating a Laravel 5.x Package :: Part 3 - Config and Asset Publishing](/creating-a-new-package-in-laravel-5-part-3-config-and-asset-publishing)
* [Creating a Laravel 5.x Package :: Part 4 - Database, Migrations and Seeding](/creating-a-new-package-in-laravel-5-part-4-database-migrations-and-seeding)
* [Creating a Laravel 5.x Package :: Part 5 - Unit Testing](/creating-a-new-package-in-laravel-5-part-5-unit-testing)
* [Demo Code on GiHub](https://github.com/websanova/laravel-demo)

## Intro

From part 2 of the series we already have quite enough setup to start building some packages. However if your package starts getting complex you will probably for one want to create some kind of configuration file. Secondly you will want a way for developers using your package to make modifications to config parameters, views and any other assets from your package.

## Config

To start we can create a `/src/config` directory to store our config files. Typically we will have only one file but I prefer this setup in case things get more complicated down the road.

**src/config/main.php**
~~~
return [
	'hello' => 'Hello',
	'world' => 'World',
];
~~~

Once we have this we will need to tell our package how to load them. We do this in the service providers `register` method.

~~~
public function register()
{
    $this->mergeConfigFrom(
        __DIR__ . '/config/main.php', 'websanova-demo-main'
    );

    ...
}
~~~

Note that we need to alias each file separately. If you're confident you will only need one config then go ahead and just use the package name like `websanova-demo` for the alias.

And a quick route to test:

~~~
Route::get('demo/config', function () {
	return config('websanova-demo-main.hello') . 
		   config('websanova-demo-main.world');
});

~~~

## Publishing Configs

This is an important part of your package as it allows developers to overwrite configs and assets in your package. Let's continue with the config and start with that first.

To begin we will add a line to the service providers `boot` method to tell it what files to publish. For the config it will look like below.

~~~
public function boot()
{
    $this->publishes([
        __DIR__ . '/config' => config_path('websanova-demo'),
    ]);

    ...
}
~~~

Fortunately here we can just publish the entire `config` directory in this case. We can then just execute the `vendor:publish` command and it will copy our files over.

~~~
> php artisan vendor:publish
~~~

If you navigate to your Laravel installation you should see the `config/websanova-demo/main.php` file now. There you can overwrite any parameters.

Note that if you keep running publish it will not replace the file. To get a fresh copy of a particular file you will need to remove the file first and then run `vendor:publish` again.

## Publishing Views

We will pretty much follow the same pattern with views. However one nice thing is that since we already aliased our views we just need to add the path to the same `publishes` method used with our config.

~~~
$this->publishes([
    __DIR__ . '/views' => base_path('resources/views/vendor/websanova-demo'),
    __DIR__ . '/config' => config_path('websanova-demo'),
]);
~~~

Just make sure to keep the alias name the same as the path. This will be important for Laravel to know where to look for a local app version of the file.

From here we can just run `vendor:publish` one more time. Again note that this will not overwrite any existing files.

~~~
> php artisan vendor:publish
~~~

## Force Publishing and Groups

When we are testing your package, particularly with migrations you may need to publish your package multiple times. You can use the `--force` option in this case. Just be careful with it's usage as it will overwrite all local changes in the app.

~~~
 > php artisan vendor:publish --force
~~~

Another useful option is to group your publishing files and tag them. This allows you to only publish that group of files and will be useful with migrations which we will cover later on.

~~~
public function boot()
{
    $this->publishes([
        __DIR__ . '/views' => base_path('resources/views/vendor/websanova-demo')
    ], 'views');

    $this->publishes([
        __DIR__ . '/config' => config_path('websanova-demo')
    ], 'config');

    ...
}
~~

Furthermore we can specify a provider to avoid publishing all packages and focus on just one particular package. Also very useful when using `--force` since we may not want to override other packages we are testing.

~~~
> php artisan vendor:publish --provider=Websanova\Demo\DemoServiceProvider --tag=config --force
~~~

It requires pretty much no extra effort so it's a good idea to organize these `publishes` methods this way. It provides that extra layer of flexibility during testing as well as for developers using your package. 

## Other Assets

You can follow the same pattern for publishing other assets like javascript, css and translation files. I won't cover the details for each here but you can take a look at [Laravel 5.x Package Docs](http://laravel.com/docs/5.0/packages) for more information as it's quite straight forward from here.

## Conclusion

We have take an extra step to your package development and covered two additional critical areas. Configs and asset publishing allowing developers to customize our packages for their own needs.