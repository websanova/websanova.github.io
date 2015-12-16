---
layout: master
title: Manually adding PHP Versions to WAMP
keywords: php, wamp, manual
description: The article details manually adding additional PHP versions to a WAMP install.
date: Jan 12 2013
permalink: /blog/php/manually-adding-php-versions-to-wamp.html
redirect_from:
  - /tutorials/php/manually-adding-php-versions-to-wamp.html
  - /manually-adding-php-versions-to-wamp.html
---

Since WAMP is usually a little behind on it's versions of Apache, MySQL and PHP we may sometimes need to install them manually.  I find this not as big an issue with Apache or MySQL but typically prefer to have my PHP version as up to date as possible.  Hence this little tutorial goes over the brief steps required to quickly update your PHP versions manually in `wamp`.

### Use 32 Bit WAMP

I had issues installing my PHP versions manually when using the 64 bit version of `wamp` and found the 32 bit version worked much better.  Installing right over the 64 bit version with the 32 bit version didn't seem to have any troubles for me and I also tested going back and forth a few times which didn't seem to create any issues.

I would recommend at least installing the latest version of `wamp` available as it is usually only a few versions behind and I find it a good idea to copy the config files straight over so that it plays nicely with `wamp`.

### Getting PHP

First you will need to download whatever version of PHP it is you are looking for:

- Latest Version: [http://windows.php.net/downloads/releases/](http://windows.php.net/downloads/releases/)
- Archived Versions: [http://windows.php.net/downloads/releases/archives/](http://windows.php.net/downloads/releases/archives/)

You will see a bunch of files in there, mostly in arrangements like so:

~~~
Thursday, December 20, 2012 12:23 AM     16507699 php-5.4.10-nts-Win32-VC9-x86.zip
Thursday, December 20, 2012 12:23 AM     16650112 php-5.4.10-Win32-VC9-x86.zip
~~~

You'll want to grab the `php-5.4.10-Win32-VC9-x86.zip` which is the `thread safe` version, the `nts` in the other file stands for `not thread safe`. 

### Installing PHP into WAMP

Once we have the zip file downloaded, we'll simply extract the contents into the `wamp/bin/php/php5.x.x` folder.  So say we downloaded `php 5.4.10` then we would put the contents into:

~~~
C:/wamp/bin/php/php5.4.10/
~~~

### Copy wampserver.conf File

Each PHP directory you install into `wamp` will have a `wampserver.conf` file.  Unless you have a really old version of `wamp` installed you will probably just be able to copy this one straight over from a previous PHP version you already have installed.

~~~
C:/wamp/bin/php/php5.4.10/wampserver.conf
~~~

Again, if you notice you are having issues, try installing the latest version of `wamp` available and grabbing the file from there.

### Copy PHP Config Files

You will need to copy two configs from your currently working PHP directory, they are called `phpForApache.ini` and `php.ini`.  These are the ones used by `wamp` and the system (CLI) respectively.  It's best to copy these over from a previous `wamp` install as they are setup in a way for `wamp` to read properly.

This will probably not produce any errors for you unless you have a really old version of PHP currently setup, but anything in the `5.3` or `5.4` family shouldn't have any issues.

You'll then need to update the `extension_dir` path to the directory of the PHP zip file you just extracted.

~~~
extension_dir = "c:/wamp/bin/php/php5.4.10/ext/"
~~~

Note that these changes should be made in both `ini` files.  Also note that if you don't copy these changes over any php changes you have made to your `ini` files will need to be copied over as well.

### Installing PECL Libraries

Sometimes you will want to install additional extensions for PHP to work with.  There is a nice set of compiled and ready to go for Windows libraries that can be found in the [pecl releases](http://windows.php.net/downloads/pecl/releases/) section of the windows.php.net website.

A more common extension typically used is the `oAuth` library which is used to communicate with sites like Facebook, Twitter and Linkedin.  I've previously written an article on [Installing PHP oAuth on Windows](/installing-php-oauth-on-windows) which covers the steps required to accomplish this.  Similar steps can be used with the other libraries there.

### Restart WAMP

In order to see the new version of PHP along with any additional extensions you have installed you will need to fully exit and then restart `wamp`.

### Update Windows Path Variables

One last update you will want to do is to update your windows `PATH` variable to point to the new version of php in order to be able to execute PHP via command line.  This will be important if you have any scripts running.  You can find the PATH variable in your environment variables and there should already be an entry for PHP there so you will just need to update the path.

In order for `PATH` environment variable changes to take effect you will need to log out of your windows session and log back in.  Once back in you can check your php is up to date by typing in `php -v` into the command line.

~~~
C:\>php -v
PHP 5.4.10 (cli) (built: May 14 2012 02:50:59)
Copyright (c) 1997-2012 The PHP Group
Zend Engine v2.3.0, Copyright (c) 1998-2012 Zend Technologies
with Xdebug v2.2.0, Copyright (c) 2002-2012, by Derick Rethans
~~~