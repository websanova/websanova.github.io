---
layout: master
title: Installing PHP oAuth on Windows
keywords: install, php, oauth, windows
description: The article details installing PHP oAuth on Windows.
date: Jan 12 2013
---

I found a lot of confusion when trying to setup the PHP `oAuth` library on Windows.  It seemed there was a lot of outdated information out there so I decided to write this little updated post on the subject.

I also noticed a lot of people having issues getting it to work with their `wamp` or `xamp` install (myself included) which required updating PHP to a version that was not yet packaged with the latest version of `wamp` or `xamp` available at the time.  So, I also wrote this little post on [Manually adding PHP Versions to WAMP](/manually-adding-php-versions-to-wamp).

It all only takes a few minutes and allows you to get up to date with all the latest and greatest goodies.

### Getting oAuth

You can get the [latest compiled PECL release of oAuth](http://windows.php.net/downloads/pecl/releases/oauth/) on the `php.net` website along with [other PECL releases](http://windows.php.net/downloads/pecl/releases/) should you need them down the road.  I found this a pretty reliable source which seems to be kept up to date quite well and worked with the latest version of PHP currently available at the time of this writing along with six previous versions of PHP that I tested.

There are many files in there, you'll want to grab the `thread-safe` version marked `ts` along with the correct version of PHP.  Say we wanted the thread safe, `oAuth` library version `1.2.3` for PHP `5.4`, then we would grab the file `php_oauth-1.2.3-5.4-ts-vc9-x86.zip`.  The `vc9` just denotes the `visual c` compiler version used to compile the `oAuth` library.

### Installing oAuth

If you're looking to get `oAuth` up and running on your existing setup you should be able to do this quite easily by just dropping the `php_oauth.dll` into your PHP's extension directory.

~~~
php5.x.x/ext/php_oauth.dll
~~~

From there you'll need to make sure you enable the `oAuth` library in your `php.ini` file.

~~~
extension=php_oauth.dll   
~~~

After adding that line you will need to restart your Apache web server for the changes to take affect.

### WAMP / XAMP

If you're using something like `wamp` or `xamp` then you will need to fully exit and restart to see `oAuth` show up in the list of extensions where you can then enable or disable it without modifying the `php.ini` file.

Also note that there are typically two `ini` files for your PHP configuration when using `wamp` or `xamp`. There will be a special one used specifically for `Apache` that in `wamp` will be called `phpForApache`. This one will auto detect the `oAuth` library however the `php.ini` is used in the CLI and may not contain these updates.  If you need to use it in command line then you may need to update it manually there.