---
layout: default
title: Install PHP on CentOS
keywords: install, php, centos
description: Article covering the installation of PHP on CentOS.
date: Sep 25 2011
---

Installing PHP is as simple as running the following yum command.

~~~
$ yum install php
~~~

If you also need support for MySQL in your PHP install then also run the following:

~~~
$ yum install php-mysql
~~~

You will probably also need to restart your web server, for Apache

~~~
$ service httpd restart
~~~