---
layout: master
title: Setting up PHP With Nginx
keywords: setting, php, nginx
description: Article covers setting up PHP with Nginx and FastCGI on CentOS.
date: Aug 25 2011
permalink: /blog/php/setting-up-php-with-nginx.html
---

This is a small tutorial on how to setup PHP with Nginx.

### Install php-fpm

Installing `php-fpm` should be simple for the most part just using yum

~~~
$ yum install php-fpm
~~~

However you may get dependency issues depending on your flavor of UNIX, or hosting provider like in my case.  You can add a repository with the necessary dependency's like so.  First create a file called `russia-repo.repo`

~~~
$ cd /etc/yum.repos.d
$ vi russia-repo.repo
~~~

Add the following into the file:

~~~
[russia-repo]
name=CentOS-$releasever Â rusia packages for $basearch
baseurl=http://centos.alt.ru/pub/repository/centos/5/i386/
enabled=1
gpgcheck=0
protect=1
~~~
You should now be able to install `php-fpm` using yum with the proper dependencies.

### Configure php-fpm

This is a small step that seemed to be overlooked on all the tutorials I saw, probably because they weren't dealing with sessions.  We need to set the user in the `php-fpm.conf` file and tell it what user to run processes as.

~~~
$ vi /etc/php-fpm.conf
~~~

Look for a few lines that look like this:

~~~
Unix user of processes
<value name="user">nobody</value>

Unix group of processes
<value name="group">nobody</value>
~~~

Change the user and group values to nginx (assuming you are using with nginx) so that it looks like this:

~~~
Unix user of processes
<value name="user">nginx</value>

Unix group of processes
<value name="group">nginx</value>
~~~

### Setup Proper Permissions

If you plan on using session, then you will have to set the proper permission on the PHP session folder.

~~~
$ ls -la /var/lib/php
drwxr-xr-x  3 root root       4096 Jan  7  2011 .
drwxr-xr-x 18 root root       4096 Aug  8 13:23 ..
drwxrwx---  2 root apache 143360 Aug  9 22:04 session
~~~

You will probably see Apache there if you already have that installed, if not just set it to Nginx or a group that has the Nginx user in it.  Remember our `php-fpm` processes are now running as Nginx user, so that is the user that needs permissions.

~~~
$ chown root:nginx /var/lib/php/session
~~~

### Configure Nginx Virtual Host

Now we have to tell Nginx to redirect PHP request to `php-fpm`, so we'll edit our virtual.conf file

~~~
$ vi /etc/nginx/conf.d/virtual.conf
~~~

Add the following to your server directive, something like the following:

~~~
server {
        listen               80;
        listen               duchnik.com:80;
        server_name     duchnik.com www.duchnik.com;
        root                 /home/webadmin/www.duchnik.com/htdocs;
        index                index.html index.htm index.php;

        location ~ \.php$ {
                root /home/webadmin/www.duchnik.com/htdocs;

                fastcgi_pass 127.0.0.1:9000;
                fastcgi_index index.php;
                include /etc/nginx/fastcgi.conf;
        }
}
~~~

### Restart Nginx and php-fpm

Now just restart Nginx and `php-fpm` and you should be good to go with PHP in Nginx.

~~~
$ service nginx restart
$ service php-fpm restart
~~~
