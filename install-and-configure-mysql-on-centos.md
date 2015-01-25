---
layout: default
title: Install and Configure MySQL on CentOS
keywords: install, configure, mysql, centos
description: Article covers installing and configuring MySQL Database Server on CentOS Linux.
date: Jun 18 2011
---

This tutorial covers installing MySQL server and client, as well as some basic configuration to get you up and running.

### Install MySQL

Install MySQL service and command line tools.

~~~
$ yum install mysql mysql-server
~~~

Once thats done turn on the MySQL service so that it automatically starts on boot.

~~~
$ chkconfig --level 2345 mysqld on
~~~

Finally start the MySQL service.

~~~
$ /etc/init.d/mysqld start
~~~

### Open up MySQL Ports

We are going to open up ports 3306 in `iptables`.

~~~
$ vi /etc/sysconfig/iptables
~~~

Add the following lines (make sure its above the reject statement).

~~~
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
-A RH-Firewall-1-INPUT -j REJECT --reject-with icmp-host-prohibited
~~~

Restart iptables service for changes to take effect.

~~~
$ /etc/init.d/iptables restart
~~~

### MySQL Client

That completes the steps to install mysql, from there you can login in through the client to setup whatever you need to.

~~~
$ mysql -u root
~~~
