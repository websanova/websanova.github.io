---
layout: master
title: Install and Configure Apache on CentOS
keywords: install, configure, apache, centos
description: The article covers installing and configuring Apache Web server on CentOS Linux.
date: Jun 16 2011
permalink: /blog/apache/install-and-configure-apache-on-centos.html
redirect_from:
  - /install-and-configure-apache-on-centos.html
---

This tutorial covers installing Apache on CentOS along with some basic configuration to get it up and running.

### Install Apache

Here we are installing Apache and SSL right away in case we ever need it.

~~~
$ yum install httpd mod_ssl
~~~

Once that's done turn on the Apache service so that it automatically starts on boot.

~~~
$ chkconfig --level 2345 httpd on
~~~

Finally we can start the Apache service.

~~~
$ /etc/init.d/httpd start
~~~

### Open HTTP/HTTPS Ports

We are going to open up ports 80 and 443 for `http` and `https` protocols in `iptables`.

~~~
$ vi /etc/sysconfig/iptables
~~~

Add the following lines (make sure its above the reject statement).

~~~
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
-A RH-Firewall-1-INPUT -j REJECT --reject-with icmp-host-prohibited
~~~

Restart the iptables service for the changes to take effect.

~~~
$ /etc/init.d/iptables restart
~~~

### Disable Security Enhanced Linux (SELinux)

This makes sure other computers can connect to the server.

~~~
$ vi /etc/sysconfig/selinux
~~~

Make sure the SELINUX setting is set to disabled

~~~
SELINUX=disabled
~~~

May require a system reboot at this point for this one to take effect.