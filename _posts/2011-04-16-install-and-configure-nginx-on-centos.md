---
layout: master
title: Install and Configure Nginx on CentOS
keywords: install, configure, nginx, centos
description: Article covers installing and configuring nginx web server on CentOS Linux.
date: Aug 16 2011
permalink: /blog/nginx/install-and-configure-nginx-on-centos.html
redirect_from: /blog/centos/install-and-configure-nginx-on-centos.html
---

Instructions on how to install and configure Nginx on CentOS.

### Install EPEL Repository

If we want to use yum to install Nginx we will need to include the `EPEL` repository.

~~~
$ rpm -Uvh http://download.fedora.redhat.com/pub/epel/5Server/x86_64/epel-release-5-4.noarch.rpm
~~~

Keep in mind, at the time of this writing, the latest `EPEL` release was `5-4`.  If you are getting any errors getting this repository, just navigate to:

~~~
http://download.fedora.redhat.com/pub/epel/5Server/x86_64/
~~~
in your browser and do a search for `epel` to find the latest version.

### Install Nginx

Now install Nginx using yum

~~~
$ yum install nginx
~~~

You may be asked to install the `EPEL gpg-key`, if so just select the `y`, yes option.

~~~
warning: rpmts_HdrFromFdno: Header V3 DSA signature: NOKEY, key ID 217521f6
Importing GPG key 0x217521F6 "Fedora EPEL <epel@fedoraproject.org>" from /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL
Is this ok [y/N]:
~~~

### Nginx Init Scripts

Once installed we will need to set up Nginx so that it automatically starts on boot.

~~~
$ chkconfig --level 2345 nginx on
~~~

You can control `Nginx` with the following commands

~~~
$ /etc/init.d/nginx start
...
$ /etc/init.d/nginx stop
...
$ /etc/init.d/nginx reload
...
$ /etc/init.d/nginx restart
~~~

You can also check current configuration with these commands:

~~~
$ /etc/init.d/nginx status
...
$ /etc/init.d/nginx configtest
~~~

### Installing Nginx From Source

I had an issue where I had to install Nginx from source due to the kernel lacking `eventfd()` support.  This was done quite simply.  First grab the latest stable Nginx release from the main site:

~~~
http://wiki.nginx.org/Install#Building_Nginx_From_Source
~~~

Then simply run the three commands as listed on the site as well.

~~~
$ ./configure
$ make
$ make install
~~~

If you have issues running `./configure` it may be because you are missing some needed packages.  If you get any errors about `pcre` or `zlib`, run the following command:

~~~
$ yum install -y httpd-devel pcre perl pcre-devel zlib zlib-devel GeoIP GeoIP-devel
~~~
Then rerun the configure/make/make commands.

Now if you have already installed Nginx you may need to update the init scripts to point to the proper nginx location.

~~~
$ vi /etc/init.d/nginx
~~~
Change the `nginx` variable from `/usr/sbin/nginx` to `/usr/local/nginx/sbin/nginx`

### Basic Nginx Configuration

For the most part you wont need to touch the main Nginx configuration which should be found under:

~~~
$ more /etc/nginx/nginx.conf
~~~
There is a file already created for you where you can edit your virtual host configurations.

~~~
$ vi /etc/nginx/conf.d/virtual.conf
~~~

You should see an example there that you can follow that looks something like this:

~~~
server {
     listen       8080;
     listen       www.domain.com:8080;
     server_name  www.domain.com domain.com;

     location / {
          root   html;
          index  index.html index.htm;
     }
}
~~~

Keep in mind, I'm just using 8080 here because I already have Apache running on port 80, this port can be changed to anything including the standard HTTP port 80.
Now just make sure your server is running:

~~~
$ service nginx start
~~~

And navigate to http://www.domain.com:8080, where you should see your site up and running.