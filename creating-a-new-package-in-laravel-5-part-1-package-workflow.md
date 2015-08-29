---
layout: default
title: Creating a Laravel 5.x Package :: Part 1 - Package Workflow
keywords: php, laravel, package, creating, development, publish, publishing, version, websanova
description: Creating a new package in Laravel can be a confusing process. In Part 1 we cover the process from setting up to publishing our packages.
permalink: /laravel
date: Aug 10 2015
---

* [Creating a Laravel 5.x Package :: Part 1 - Package Workflow](/creating-a-new-package-in-laravel-5-part-1-package-workflow)
* [Creating a Laravel 5.x Package :: Part 2 - Controllers, Routes and Views](/creating-a-new-package-in-laravel-5-part-2-controllers-routes-and-views)
* [Creating a Laravel 5.x Package :: Part 3 - Config and Asset Publishing](/creating-a-new-package-in-laravel-5-part-3-config-and-asset-publishing)
* [Creating a Laravel 5.x Package :: Part 4 - Database, Migrations and Seeding](/creating-a-new-package-in-laravel-5-part-4-database-migrations-and-seeding)
* [Creating a Laravel 5.x Package :: Part 5 - Unit Testing](/creating-a-new-package-in-laravel-5-part-5-unit-testing)
* [Demo Code on GiHub](https://github.com/websanova/laravel-demo)

# Creating a Laravel 5.x Package :: Part 1 - Package Workflow

## Intro

I see lots of tutorials jumping right into development, but let's start with some simple setup. You'll probably want to keep your project in source control and although you may develop for current version 5.1.x for instance. The speed at which Laravel is updating means you will probably need to test for multiple versions in the near future. A few questions you will probably ask yourself.

* Where do I keep all my packages?
* How to deal with multiple repositories (GitHub)?
* What about different versions of Laravel?
* What about Lumen?
* What do I need in that composer.json file?
* How do I publish?

I have yet to find a comprehensive guide on Laravel packages that covers the full process from beginning to end. In this first part we will cover setting up and aim to answer the questions above. In part 1 we will not cover any actual Laravel 5 package development. We will focus on workflow and getting things setup properly first. It actually only takes a few minutes and is a one time thing so let's get setup and move onto coding.

## Folder Structure

First off let's create our folders. Keep in mind we want to take in account multiple packages all with their own repository and different versions of Laravel.

~~~
./laravel-packages/laravel/4.x.x
                  /laravel/5.0.x
                  /laravel/5.1.x
                  /laravel/...
                  /lumen/5.1.x
                  /lumen/...
                  /packages/websanova/demo
                  /packages/websanova/easy-cache
                  /packages/foo/bar
                  /...
~~~

This lets us test in multiple clean versions of Laravel and Lumen. In each of the version folders we will pull in the corresponding Laravel or Lumen version.

~~~
composer create-project laravel/laravel ./laravel/5.1.x 5.1
composer create-project laravel/lumen ./lumen/5.1.x 5.1
~~~

Once we have our Laravel (or Lumen) version pulled in we will `symlink` our packages directory into the folder.

**Windows:**
* Make Sure to run command prompt as Admin if you have UAC enabled.
* Make sure you use full paths in Windows and not relative paths.
* In Windows it's target then source.
~~~
mklink /d "C:\target\path\laravel-packages\laravel\5.1.x\packages" "C:\source\path\laravel-packages\packages"
~~~

**Linux**
~~~
ln -ls /source/path/laravel-packages/packages /target/path/laravel-packages/5.1.x/packages
~~~

## Repositories (GitHub)

Now that we have our packages folders setup we can setup Git. This just follows from the folder structure above. Since our packages folder is already symlinked we now have one central location for all our package repositories.

## Server

Since we are just testing a package we don't have to worry about servers. We will just use the php server that comes conveniently packaged with Laravel. Navigate to whatever version of Laravel you want to test and fire up the server.

~~~
php artisan serve
~~~

We can also but multiple servers using the `--port` flag.

~~~
php artisan serve --port 8001
~~~

## composer.json

For now we will just go with the minimum configuration required for the `composers.json` file. We will add to it as we go along with our packages development later on.

Note that this is the packages composer.json file, which has nothing to do with the Laravel/Lumen composer.json file. This file should be located at the root of your packages directory.

~~~
./laravel-packages/packages/websanova/demo/composer.json
~~~

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

}
~~~

It's highly recommended you include the keyword `laravel` in the keywords section. You can find more information on the [packagist.org](https://packagist.org) website.

## Publishing

Although we don't have any working code yet we can go ahead and publish the package to finish up our workflow. If you don't already have an account you will need to register at [Packagist.org](https://packagist.org).

Once registered it's as easy as submitting your GitHub link on the [Packagist Submit](https://packagist.org/packages/submit) page. As long as you have a valid `composer.json` file at the root of your project, Packagist will scan it and automatically setup the project for you.

From there you can visit your packages Packagist.org page and hit update anytime you need to update the package. You can also setup up a commit hook to auto publish when ever you push code up to your repository. More information on setting up the hook can be found on the [Packagist.org How to Update Packages](https://packagist.org/about#how-to-update-packages) page.

## Package Versions

Package versions are the easiest part and follow standard convention in GitHub for releasing versions. As it's already well documented you can visit the [GitHub Creating Releases](https://help.github.com/articles/creating-releases/) page for more information.

## Conclusion

There you have it. With a few simple steps we're properly setup to start working on our package in only a few minutes. We can now easily test in multiple versions of Laravel and Lumen and maintain multiple packages in separate repositories. We also know how to version and publish our package for the others to start using.
