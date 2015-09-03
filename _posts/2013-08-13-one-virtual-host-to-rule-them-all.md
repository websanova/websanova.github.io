---
layout: master
title: One Virtual Host to Rule Them All
keywords: apache, virtual, host, virtualhost, websanova 
description: We have quite a few plugins on Websanova now and it was becoming cumbersome to constantly add new VirtualHost directives for each new plugin. We wanted to automate and streamline this a little bit so that we could just drop in a plugin folder and automatically have it served by Apache with no additional configuration.
date: Aug 13 2013
permalink: /blog/apache/one-virtual-host-to-rule-them-all
---

Had a bit of head scratching on this one this morning so thought I would share. We have quite a few [plugins](/plugins) on Websanova now and it was becoming cumbersome to constantly add new `VirtualHost` directives for each new plugin. We wanted to automate and streamline this a little bit so that we could just drop in a plugin folder and automatically have it served by Apache with no additional configuration. This can be achieved by using the `VirtualDocumentRoot container rather than the `DocumentRoot` container in what is known as [dynamically configured mass virtual hosting](http://httpd.apache.org/docs/2.2/vhosts/mass.html). Setting this is up is really simple once you figure out the syntax and it’s quite effective.

## Setup

First you will need to enable the [mod_vhost_alias](http://httpd.apache.org/docs/2.2/mod/mod_vhost_alias.html) Apache module which recognizes the `VirtualDocumentRoot` directive. After that all we need to do is to setup a wildcard `ServerAlias` and our `VirtualDocumentRoot`

~~~
<VirtualHost *:80>
    ServerName websanova.com
    ServerAlias *.websanova.com
    VirtualDocumentRoot "/path/to/plugins/%2+"
</VirtualHost>
~~~

## Gotchas

It seems the `VirtualDocumentRoot` does not like . characters in it’s path. Having one in the path didn’t even allow my Apache server to start. I was not able to figure out how to escape it and will update if I do. If you have any . characters in your path this may pose a problem. Otherwise if it’s just your domain as the path that contains dot characters than you will be fine. Make sure to read the [mod_vhost_alias](http://httpd.apache.org/docs/2.2/mod/mod_vhost_alias.html) page on how the the `%` works, but here is the basic run down from the Apache page.

~~~
%%         insert a %
%p         insert the port number of the virtual host
%N.M       insert (part of) the name
0          the whole name
1          the first part
2          the second part
-1         the last part
-2         the penultimate part
2+         the second and all subsequent parts
-2+        the penultimate and all preceding parts
1+ and -1+ the same as 0
~~~

## Not Found

What happens if a directory for the domain doesn’t exist. Well if you have a domain configured on your server you will get a `404 Not Found` error from your web server. If you don’t have the domain configured you will simply get an error from your browser saying the domain is not found. So you probably shouldn’t have any issues here as long as you make sure a domain is set only for directories that exist.

## Before

~~~
<VirtualHost *:80>
    ServerName wpaint.websanova.com
    DocumentRoot "/path/to/plugins/wpaint.websanova.com"
</VirtualHost>

<VirtualHost *:80>
    ServerName url.websanova.com
    DocumentRoot "/path/to/plugins/url.websanova.com"
</VirtualHost>

etc...
~~~

## After

~~~
<VirtualHost *:80>
    ServerName websanova.com
    ServerAlias *.websanova.com
    VirtualDocumentRoot "/path/to/plugins/%2+"
</VirtualHost>
~~~

As you can see this is much cleaner and easier to manage. For simple things where we don’t need any specific configuration this works great.