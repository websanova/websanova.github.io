---
layout: master
title: "Creating a Laravel 5.x Package :: Part 5 - Unit Testing"
keywords: php, laravel, package, unit, testing, websanova
description: In Part 5 we cover a neglected area in package development. If we want others contributing fixes and updates to our package then unit testing will become crucial.
permalink: /blog/laravel/creating-a-new-package-in-laravel-5-part-5-unit-testing.html
date: Aug 29 2015
---

* [Part 1 - Package Workflow](/blog/laravel/creating-a-new-package-in-laravel-5-part-1-package-workflow)
* [Part 2 - Controllers, Routes and Views](/blog/laravel/creating-a-new-package-in-laravel-5-part-2-controllers-routes-and-views)
* [Part 3 - Config and Asset Publishing](/blog/laravel/creating-a-new-package-in-laravel-5-part-3-config-and-asset-publishing)
* [Part 4 - Database, Migrations and Seeding](/blog/laravel/creating-a-new-package-in-laravel-5-part-4-database-migrations-and-seeding)
* [Part 5 - Unit Testing](/blog/laravel/creating-a-new-package-in-laravel-5-part-5-unit-testing)
* [Demo Code on GiHub](https://github.com/websanova/laravel-demo)

## Intro

In this final part of the series we cover perhaps the most important and often overlooked part of package development. Unit testing ensures greater stability as we and others improve and update the package. Without even some basic unit testing it makes it hard to maintain the package as it evolves over time.

## Install `phpunit`

We will need to install `phpunit` into each of our Laravel install directories. Let's open the `composer.json` file in whatever version of Laravel you want to run the tests from.

~~~
"require-dev": {
    ...
    "phpunit/phpunit": "4.8.*"
},
~~~

The version at the time of this writing was `4.8.1`, but you can just check [Packagist  phpunit](https://packagist.org/packages/phpunit/phpunit) page for the latest version. Once we add this we'll need to do a `composer update`.

~~~
> composer update phpunit/phpunit
~~~

## Create `phpunit.xml`

The next step is to create the `phpunit.xml` file in the root of our package. We can just use the sample below to start but of course you can put in your won configuration as well.

~~~
#packages/websanvoa/demo/phpunit.xml

<?xml version="1.0" encoding="UTF-8"?>
<phpunit backupGlobals="false"
         backupStaticAttributes="false"
         colors="true"
         convertErrorsToExceptions="true"
         convertNoticesToExceptions="true"
         convertWarningsToExceptions="true"
         processIsolation="false"
         stopOnFailure="false"
         syntaxCheck="false"
>
    <testsuites>
        <testsuite name="Package Test Suite">
            <directory suffix=".php">./tests/</directory>
        </testsuite>
    </testsuites>
</phpunit>
~~~

## Test Files

We will then also create a `tests` directory in the root of our package. In there we will house our test files.

~~~
#packages/websanova/demo/tests/DemoTest.php

class DemoTest extends TestCase
{
    public function testSomethingIsTrue()
    {
        $this->assertTrue(true);
    }
}
~~~

## Running `phpunit`

Now we will need to run `phpunit` from the packages directory we want to test from.

~~~
> cd /path/to/packages/websanova/demo
> ../../../vendor/bin/phpunit
~~~

We do it this way so that we can include the `phpunit.xml` file for our package which may have it's own specific configuration. This also makes it easier to distribute the package for other developers who may wish to contribute allowing them to use their own workflows.

## Conclusion

We didn't get into specific testing concepts as there is a lot of great material on the web for that topic already. The idea was just to get your tests setup with your package.

This the end of the series and pretty much wraps up package development. Hopefully you will now have all the tools you need to build your own packages.