---
layout: default
title: Install Mailman on CentOS
keywords: install, mailman, centos
description: Article covers installing Mailman mail service on CentOS Linux.
date: Jun 6 2011
---

This tutorial covers installing Milman on CentOS along with a guide on how to setup and create your first mailing list.

### Install Mailman

Installing mailman from yum.

~~~
$ yum install mailman
~~~

Once thats done turn on the Mailman service so that it automatically starts on boot.

~~~
$ chkconfig --level 2345 mailman on
~~~

Finally we can start the Mailman service.

~~~
$ /etc/init.d/mailman start
~~~

Set root password for Mailman administration.  You will need this to access the administration pages of the mailman GUI as well as creating or modifying lists.

~~~
$ /usr/lib/mailman/bin/mmsitepass <password>
~~~

### Create List

To create a list simply type the following.

~~~
$ /usr/lib/mailman/bin/newlist <listname>
~~~

You will be prompted to enter an administrative email and a password for the list.

~~~
Enter the email of the person running the list:
Initial mailing-list password:
~~~

Once the list is created you will get a message telling you to add some entries in the `/etc/aliases` file.

~~~
To finish creating your mailing list, you must edit your /etc/aliases (or
equivalent) file by adding the following lines, and possibly running the
'newaliases' program:

## mailing-list mailing list
mailing-list:              "|/usr/lib/mailman/mail/mailman post mailing-list"
mailing-list-admin:        "|/usr/lib/mailman/mail/mailman admin mailing-list"
mailing-list-bounces:      "|/usr/lib/mailman/mail/mailman bounces mailing-list"
mailing-list-confirm:      "|/usr/lib/mailman/mail/mailman confirm mailing-list"
mailing-list-join:         "|/usr/lib/mailman/mail/mailman join mailing-list"
mailing-list-leave:        "|/usr/lib/mailman/mail/mailman leave mailing-list"
mailing-list-owner:        "|/usr/lib/mailman/mail/mailman owner mailing-list"
mailing-list-request:      "|/usr/lib/mailman/mail/mailman request mailing-list"
mailing-list-subscribe:    "|/usr/lib/mailman/mail/mailman subscribe mailing-list"
mailing-list-unsubscribe:  "|/usr/lib/mailman/mail/mailman unsubscribe mailing-list"
~~~

Go in to the /etc/aliases file and add the entries then do an aliases update by running the following:

~~~
$ newaliases
~~~

Once your list is created you can administer it through the mailman interface, which is the preferred method.  You can create lists through the interface as well, but I have had trouble with the aliases and have had to still create them manually afterwards.

~~~
http://<domain>/mailman/listinfo
~~~

### Remove List

To remove a list type the following.

~~~
$ /usr/lib/mailman/bin/rmlist <listname>
~~~

If you want to also remove all existing archives of the mailing list then use the -a option.

~~~
$ /usr/lib/mailman/bin/rmlist -a <listname>
~~~

You will get a message telling you to remove some entries in the `/etc/aliases` file.

~~~
To finish removing your mailing list, you must edit your `/etc/aliases` (or
equivalent) file by removing the following lines, and possibly running the
`newaliases' program:

## test-mailing-list mailing list
test-mailing-list:              "|/usr/lib/mailman/mail/mailman post test-mailing-list"
test-mailing-list-admin:        "|/usr/lib/mailman/mail/mailman admin test-mailing-list"
test-mailing-list-bounces:      "|/usr/lib/mailman/mail/mailman bounces test-mailing-list"
test-mailing-list-confirm:      "|/usr/lib/mailman/mail/mailman confirm test-mailing-list"
test-mailing-list-join:         "|/usr/lib/mailman/mail/mailman join test-mailing-list"
test-mailing-list-leave:        "|/usr/lib/mailman/mail/mailman leave test-mailing-list"
test-mailing-list-owner:        "|/usr/lib/mailman/mail/mailman owner test-mailing-list"
test-mailing-list-request:      "|/usr/lib/mailman/mail/mailman request test-mailing-list"
test-mailing-list-subscribe:    "|/usr/lib/mailman/mail/mailman subscribe test-mailing-list"
test-mailing-list-unsubscribe:  "|/usr/lib/mailman/mail/mailman unsubscribe test-mailing-list"
~~~

Go in to the `/etc/aliases` file and remove the entries then do an aliases update by running the following:

~~~
$ newaliases	
~~~

A good resource covering all the features of mailman can be found here: