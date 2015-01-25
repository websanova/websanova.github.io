---
layout: default
title: Setup and Use iostat
keywords: setup, use, iostat, centos
description: Article covers setting up an using iostat to gather performance statistics on CentOS Linux.
date: Jul 18 2011
---

The `iostat` utility is used to gather performance statistics for partitions or devices.  You may need to install the `sysstat` package first before you use the `iostat` command.

### Install iostat

~~~
$ yum install sysstat
~~~

To run `iostat` simply type:

~~~
$ iostat
~~~

### Basic Usage

You can tell `iostat` to run continuously by setting the an interval, in minutes and number of intervals to run.  The below example specifies an interval of 30 seconds to be run 10 times.

~~~
$ iostat 30 10
~~~

You can also just look at one device at a time by using the `-p <device>` option.

~~~
$ iostat -p /dev/sda
~~~

To display only CPU usage use the -c option and to display only the device utilization report use the -d option.

~~~
$ iostat -c
$ iostat -d
~~~

To display the information in kilobytes per second rather then blocks per second use the -k option.

~~~
$ iostat -k
~~~

To display extended statistics use -x option.

~~~
$ iostat -x
~~~

### Examples:

Display a continuous device report at two second intervals.

~~~
$ iostat -d 2
~~~

Display six reports at two second intervals for all devices.

~~~
$ iostat -d 2 6
~~~

Display six reports of extended statistics at two second  intervals for devices `hda` and `hdb`.

~~~
$ iostat -x hda hdb 2 6
~~~

Display  six  reports at two second intervals for device `sda` and all its partitions (`sda1`, etc.)

~~~
$  iostat -p sda 2 6
~~~
