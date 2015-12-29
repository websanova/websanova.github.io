---
layout: master
title: "Creating a Laravel 5.x Package :: Part 6 - Misc"
keywords: php, laravel, package, misc, sqlite
description: In Part 6 we discuss some extra features of package development in Laravel. Some of these are more general concepts but will aid you when developing Laravel pacakges.
date: Dec 29 2015
img: /wp-content/uploads/2015/creating-a-new-package-in-laravel-5.png
permalink: /blog/laravel/creating-a-new-package-in-laravel-5-part-6-misc.html
---

* [Part 1 - Package Workflow](/blog/laravel/creating-a-new-package-in-laravel-5-part-1-package-workflow)
* [Part 2 - Controllers, Routes and Views](/blog/laravel/creating-a-new-package-in-laravel-5-part-2-controllers-routes-and-views)
* [Part 3 - Config and Asset Publishing](/blog/laravel/creating-a-new-package-in-laravel-5-part-3-config-and-asset-publishing)
* [Part 4 - Database, Migrations and Seeding](/blog/laravel/creating-a-new-package-in-laravel-5-part-4-database-migrations-and-seeding)
* [Part 5 - Unit Testing](/blog/laravel/creating-a-new-package-in-laravel-5-part-5-unit-testing)
* [Part 6 - Misc](/blog/laravel/creating-a-new-package-in-laravel-5-part-6-misc)
* [Demo Code on GiHub](https://github.com/websanova/laravel-demo)

## Sqlite on Homestead

If you're using Homestead for Laravel and you decided to create a package you may be wondering how can I access my sqlite database inside of homestead. Fortunately there is a convenient drop in script we can use for this.

* [PHPLiteAdmin](http://www.phpliteadmin.org)

You can gab the latest version and just drop the `phpliteadmin.php` and the `phpliteadmin.config.php` files in the `public` folder. There is not much you need to change in the config file.

~~~
$password = 'homestead';

$directory = '../storage';

$databases = array(
	array(
		'path'=> 'database.sqlite',
		'name'=> 'Homestead'
	)
);
~~~

Really you just need to set the `$directory` and `$databases` variables and you're good to go. For developing packages locally this is great. Now just navigate to your wherever you are serving your app from.

~~~
http://192.168.10.10:8000/phpliteadmin.php
~~~