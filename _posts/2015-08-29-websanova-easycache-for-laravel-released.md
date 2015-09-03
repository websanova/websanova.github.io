---
layout: master
title: Websanova EasyCache for Laravel Released
keywords: websanova, easycache, easy, cache, laravel, laravel5, laravel4, 5.1, 5.0
description: Websanova EasyCache Released for Laravel version 5.x. EasyCache is a simple on demand caching extension for laravel similar to get() and find(). It extends Laravel to also allow cache().
date: Aug 29 2015
permalink: /blog/laravel/websanova-easycache-for-laravel-released.html
---

I've been using Laravel since version 4 and have been loving it. It is my framework and general solution of choice for all my projects moving forward.

In a couple of my pet projects I have built up a simple on demand caching mechanism which I have now released as an open source package for Laravel.

It works similar to Laravel's `get()` and `find()` methods but is now extended to also include `cache()`.

### Examples:

~~~
$item = Item::cache($id);            // Get via integer.
$item = Item::cache($val);           // Get via string value.
$item = Item::cache($email, 'email') // Get from cache via email `$passthrough`.

$items = Item::cache();         // Paginated, using `$perPage`.
$items = Item::cache(true, 40); // Paginated with custom.
$items = Item::cache(false);    // No pagination (`get`).
~~~

There are more features that you can check out in the documentation on the official page on GitHub.

**GitHub:** [https://github.com/websanova/easycache](https://github.com/websanova/easycache)

**Packagist:** [https://packagist.org/packages/websanova/easycache](https://packagist.org/packages/websanova/easycache)

Hope you enjoy, if you have any issues just leave me a note on GitHub.
