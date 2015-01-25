---
layout: default
title: Install and Configure NTP on CentOS
keywords: install, configure, ntp, centos
description: Article covers installing and configuring NTP time system on CentOS Linux.
date: Jul 4 2011
---

This is a simple tutorial on how to install and configure the NTP service to make sure your clock is accurate and synched on your system.

### Install NTP

~~~
$ yum install ntp
~~~

Once that's done turn on the NTP service so that it automatically starts on boot.

~~~
$ chkconfig --level 2345 ntpd on
~~~

### Synchronize Clock

Here we are just setting the service the NTP server will use to synchronize the clock with, in this case we are using the `0.pool.ntp.org` server

~~~
$ ntpdate pool.ntp.org
~~~

### Start NTP Service

~~~
$ /etc/init.d/ntpd start
~~~
