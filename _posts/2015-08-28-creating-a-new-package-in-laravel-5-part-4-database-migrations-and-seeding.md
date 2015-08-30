---
layout: master
title: "Creating a Laravel 5.x Package :: Part 4 - Database Migrations and Seeding"
keywords: php, laravel, package, creating, database, migration, seeding, models, websanova
description: In Part 4 we cover setting up our database. We then get into migrations and seeding data between our package and an app.
permalink: /blog/laravel/creating-a-new-package-in-laravel-5-part-4-database-migrations-and-seeding.html
date: Aug 28 2015
---

* [Part 1 - Package Workflow](/blog/laravel/creating-a-new-package-in-laravel-5-part-1-package-workflow)
* [Part 2 - Controllers, Routes and Views](/blog/laravel/creating-a-new-package-in-laravel-5-part-2-controllers-routes-and-views)
* [Part 3 - Config and Asset Publishing](/blog/laravel/creating-a-new-package-in-laravel-5-part-3-config-and-asset-publishing)
* [Part 4 - Database, Migrations and Seeding](/blog/laravel/creating-a-new-package-in-laravel-5-part-4-database-migrations-and-seeding)
* [Part 5 - Unit Testing](/blog/laravel/creating-a-new-package-in-laravel-5-part-5-unit-testing)
* [Demo Code on GiHub](https://github.com/websanova/laravel-demo)

## Intro

For part 3 of the series we will cover some basic migrations and seeding as it pertains to package development.

Some packages will require a database but sometimes we just need it for our own local testing while developing a package. In particular if it's a package that extends Eloquent or other database operations.

For package testing I prefer to just use `sqlite` as I find it's easites to use for isolated local testing. However, if you're using eloquent really it should work on all supported databases. It's a good idea to document this in your packages README file in case there are issues.

## Setting up the DB

There is not much out of the ordinary here, you will setup your connection as normal. If you are using `sqlite` then you may need to create a `storage/database.sqlite` file.

Also since the Laravel we will be using is just for package testing we can remove the existing `user` migration files.

~~~
> rm ./database/migrations/2014_10_12_000000_create_users_table.php
> rm ./database/migrations/2014_10_12_100000_create_password_resets_table.php
~~~

From there we can just run a `migrate` to make sure the connection is working correctly. You should see the `Nothing to migrate.` message.

~~~
> php artisan migrate
> Nothing to migrate.
~~~

## Migration Files

Next we'll just create a `src/migrations` directory for our migration files. It's good idea to prefix your migration file names and tables to avoid conflicts.

**src/migrations/0000_00_00_000000_create_websanova_demo_items_table.php**
~~~
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateWebsanovaDemoItemsTable extends Migration
{
	public function up()
	{
		Schema::create('websanova_demo_items', function(Blueprint $t)
		{
			$t->increments('id')->unsigned();
			$t->text('slug', 255);
			$t->text('name', 255);
			$t->text('description', 255);
			$t->timestamps();
		});
	}

	public function down()
	{
		Schema::drop('websanova_demo_items');
	}
}
~~~

For the migration file names you must provide the proper format with `YYYY_MM_DD_TTTTTT_FILENAME`. This will be necessary for `migrate` to parse the file properly. For the most part you can just put `0` there. If you need to run them in a specific sequence you can add appropriate values there.

There are two options for publishing migration files. The first is like we have been doing using the `publishes` method.

~~~
public function boot()
{
    $this->publishes([
        __DIR__ . '/migrations' => $this->app->databasePath() . '/migrations'
    ], 'migrations');

    ...
}
~~~

Generally you will want to set it up this way if developers need to have that as part of their app. 

~~~
> php artisan vendor:publish --tag=migrations
~~~

Again we could use our `--force` and `--provider` flags as necessary.

If the tables are strictly used for package testing then I would not add this method. In this case we can just run it directly use `--path` option.

~~~
> php artisan migrate --path=/packages/websanova/demo/src/migrations
~~~

**NOTE:** There is a small caveat here regarding `rollback` in this case. Since the migration files are not actually in the app we will need to add the migrations path in the `composer.json` file.

~~~
"autoload": {
    "classmap": [
        "database",
        "packages/websanova/demo/src/migrations"
    ],
    ...
~~~

This of course is not necessary if you are publishing the migrations directly into the app's `migrations` folder.

To reiterate if the migration files are strictly for testing the package and are not actually needed within the app then we want to avoid publishing any files when the user runs `vendor:publish`.

## Seeding Files

First we will create `src/seeds` folder and follow the same naming conventions for our files.

**src/seeds/DemoSeeder.php**
~~~
namespace Websanova\Demo;

use Illuminate\Database\Seeder;

class DemoSeeder extends Seeder
{
    public function run()
    {
        DB::table('websaanova_demo_items')->insert([
        	'slug' => 'test',
        	'name' => 'Test',
        	'description' => 'My first item test.',
        	'created_at' => \Carbon\Carbon::now(),
            'updated_at' => \Carbon\Carbon::now()
        ]);
    }
}
~~~

You probably won't use an `insert` call but we won't get into the details of seeding here. You can follow whatever regular practices you use for seeding. Here we will just illustrate how to run the package seeding file.

For seeding we don't need to publish the files. We will specify which class to run using the `class` option.

~~~
> php artisan db:seed --class=Websanova\Demo\Seeds\DemoSeeder
~~~

The only hassle here is that we can't run `migrate:refresh --seed` in one line. We will need to run them separately.

~~~
> php artisan migrate:rollback
> php artisan migrate --path=/packages/websanova/demo/src/migrations
> php artisan db:seed --class=Websanova\Demo\Seeds\DemoSeeder
~~~

## Models

Let's wrap things up by retrieving some data from a model and displaying it. We'll create a `src/models` folder to keep our models in.

**/src/models/Item.php**
~~~
namespace Websanova\Demo\Models;

use Illuminate\Database\Eloquent\Model;

class Item extends Model
{
    protected $table = 'websanova_demo_items';
}
~~~

Then we create our view in our `src/Http/routes.php` file.

~~~
Route::get('demo/model', function () {
	dd(\Websanova\Demo\Models\Item::get());
});
~~~

We should see this successfully run with or without data.

## Conclusion

We have covered a lot in part 4 on migrations and seeding. These will be necessary if we are building full blown packages that integrate right into our app, like some analytics back end or shopping cart.