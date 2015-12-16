---
layout: master
title: Wordpress MySQL Setup Commands
keywords: wordpress, mysql, setup, command
description: Article covering the necessary MySQL commands to execute in order to setup wordpress.
date: Oct 3 2011
permalink: /blog/mysql/wordpress-mysql-setup-commands.html
redirect_from:
  - /wordpress-mysql-setup-commands.html
---

I do this so often that I wanted to create a little reference tutorial of MySQL command needed to get your WordPress install up and running.

### MySQL Commands
First we are gonna create the database for this WordPress install.  I like to prefix the database name with `wp_` so I know its a WordPress database, followed by the domain name so I know what website it is referring to.  I also like to setup a specific user just for the WordPress install so I use the same naming for that user.

~~~
mysql> create database wp_domain;
mysql> GRANT ALL PRIVILEGES ON wp_domain.* TO 'wp_domain'@'localhost' IDENTIFIED BY 'password';
~~~

### Configure WordPress Config File

Next we just configure the WordPress `wp-config` file.  First we will need to rename the sample file found in the root directory of the WordPress install.  We'll just copy it over and keep the original one just in case.

~~~
$ cd /path/to/wordpress
$ cp wp-config-sample.php wp-config.php
~~~

Next we'll just need to modify the three constants at the top for DB_NAME, DB_USER, and DB_PASSWORD which should all come from the values up top.

~~~
$ vi wp-config.php

define('DB_NAME', 'wp_domain');
define('DB_USER', 'wp_domain');
define('DB_PASSWORD', 'password);
~~~