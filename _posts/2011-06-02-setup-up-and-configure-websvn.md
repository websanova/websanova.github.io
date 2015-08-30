---
layout: master
title: Setup and Configure WebSVN
keywords: setup, configure, websvn, centos, linux
description: Article covers setting up websvn on CentOS Linux for use with svn (Subversion).
date: Jun 2 2011
permalink: /blog/svn/setup-up-and-configure-websvn.html
---

WebSVN is a popular web based repository browser that is a useful tool to use to view the contents of your repository.  It only takes a few minutes to setup so I wanted to quickly cover it here.

### Get WebSVN

First we're just going to check what the latest version of WebSVN is.

~~~
$ svn list http://websvn.tigris.org/svn/websvn/tags
1.00/
...
2.3.2/
~~~

You will get a long list of tags here, get the latest version and export it.  At the time of this writing the latest version is 2.3.2, so we will just run the following.

~~~
$ svn export http://websvn.tigris.org/svn/websvn/tags/2.3.2 /path/to/websvn
~~~

### Setup Apache

We are going to add the following lines to our Apache configuration to serve WebSVN.

~~~
Alias /websvn "/path/to/websvn"

<Directory "/path/to/websvn">
    Options -Indexes MultiViews
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>
~~~

Restart apache for the changes to take affect.

~~~
$ service httpd restart
~~~

### Set Path For the Repository

If you go into the websvn folder, you will see a file in there called `distconfig.php`, you will have to copy this over to a file called `config.php` that websvn will look for by default.

~~~
$ cp /path/to/websvn/include/distconfig.php /path/to/websvn/include/config.php
~~~

We will need to add a line to the websvn configuration so that it knows where our repository is.  This assumes we already have a repository setup somewhere [Install and Configure SVN](/install-and-configure-svn-on-centos).

~~~
$ vi /path/to/websvn/include/config.php
~~~

Then add a line near the remote repositories section like so:

~~~
$config->addRepository('My Repo', 'http://&amp;lt;domain>/svn', NULL, '<username>', '<password>');
~~~

Note that you can add a line like the one above for each repository you have.

### Setup Access to WebSVN

At this point we should be able to access websvn using:

~~~
http://<domain>/websvn
~~~ 
 
We will want to setup some access to it though, so that not just anyone can visit our repository.  We've already prepared this through our Apache configuration from above, so now we just have to add a `.htaccess` files with some users.  Create the following file in the root of the websvn folder.

~~~
$ vi /path/to/websvn/.htaccess
~~~

Add the following lines to it:

~~~
AuthName "Websvn Login"
AuthType Basic
AuthUserFile /path/to/websvn/.htpasswd
Require valid-user
~~~

Then create some users for access to it.  If you have already followed the tutorial on [Install and Configure SVN](/install-and-configure-svn-on-centos), then you could just specify the path to the `passwd` file there and skip the next two steps.

~~~
$ htpasswd -c /home/www/websvn/.htpasswd <username>
New password: 
Re-type new password: 
Adding password for user <username>
~~~

Add some users to it:   

~~~
$ htpasswd /home/www/websvn/.htpasswd <username>
~~~
After setting up the `.htaccess` file, your users will be prompted by simple username/password dialog that they will have to fill out in order to view the contents of the repository.
