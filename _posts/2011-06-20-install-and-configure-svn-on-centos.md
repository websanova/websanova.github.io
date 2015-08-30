---
layout: master
title: Install and Configure SVN on CentOS
keywords: install, configure, svn, centos
description: Article covers installing and configuring SVN (subversion) on CentOS.
date: Jun 20 2011
permalink: /blog/svn/install-and-configure-svn-on-centos.html
---

This tutorial will cover the step by step instructions necessary to install SVN on CentOS and configure it to work with Apache.

### Install SVN

Install `mod_dav_svn` right away which is required to make SVN work with Apache.

~~~
$ yum install mod_dav_svn subversion
~~~

Once we have installed svn we want to create a repository somewhere.

~~~
$ svnadmin create /path/to/<repo_name>
~~~

Since we will be accessing the repository through Apache, we need to make sure the Apache user has proper permissions to access the repository.

~~~
$ chown -R apache:apache /path/to/<repo_name>
~~~

### Setup SVN Users

We are going to make some simple modifications to the configuration file for the repository to allow users access to it.

~~~
$ vi /path/to/<repo_name>/conf/svnserve.conf
~~~

Uncomment the lines like so:	

~~~
auth-access = write
password-db = passwd
~~~

Create a `htpasswd` file which will contain usernames and passwords that your users will use to gain access to the repository.

~~~
$ htpasswd -c /path/to/<repo_name>/conf/passwd <username>
New password: 
Re-type new password: 
Adding password for user <username>
~~~	

You can continue to add more users to the file with the following command:	

~~~
$ htpasswd /path/to/<repo_name>/conf/passwd <username>
~~~

### Configure Apache

Here is where the mod_dav_svn module will come into play.  We are going to setup an Apache config file specifically for subversion that will give us `http` access to the repository.  Typically your Apache configuration will have a line like the following that includes all configuration files in the `conf.d` folder, if not just add the following line:

~~~
Include conf.d/*.conf
~~~	

Once that is taken care of create the following file:	

~~~
$ vi /etc/httpd/conf.d/subversion.conf
~~~

Add the following into the configuration, note that the path `/svn` is where the repository will be served out of:	

~~~
LoadModule dav_svn_module     modules/mod_dav_svn.so
LoadModule authz_svn_module   modules/mod_authz_svn.so

<Location /svn>
    DAV svn
    SVNPath /path/to/<repo_name>
    Authtype Basic
    AuthName "My Repository"
    AuthUserFile /path/to/<repo_name>/conf/passwd
    Require valid-user
</Location>
~~~

Restart apache for the new configuration to take affect:

~~~
$ service httpd restart
~~~	

### Access the Repository

Now we simply type the address of where we are hosting the repository into our browser and we should be all set.

~~~
http://<domain>/svn
~~~

### Multiple Repositories

Note that this process can be repeated for multiple repositories on the same server, just make sure the paths in your `<Location>` directive are different.  So for instance our `subversion.conf` could look something like this:	

~~~
LoadModule dav_svn_module     modules/mod_dav_svn.so
LoadModule authz_svn_module   modules/mod_authz_svn.so

<Location /svn>
    DAV svn
    SVNPath /path/to/<repo_name>
    Authtype Basic
    AuthName "My Repository"
    AuthUserFile /path/to/<repo_name>/conf/passwd
    Require valid-user
</Location>

<Location /svn2>
    DAV svn
    SVNPath /path/to/<repo_name2>
    Authtype Basic
    AuthName "My Repository 2"
    AuthUserFile /path/to/<repo_name>/conf/passwd
    Require valid-user
</Location>	
~~~

Note that we are using the same `passwd` file from the first repository, there is no need to create a separate set of users if you don't have to.