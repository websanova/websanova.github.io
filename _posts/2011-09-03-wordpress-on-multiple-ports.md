---
layout: master
title: Wordpress on Multiple Ports
keywords: wordpress, multiple, ports
description: Article covers setting up wordpress on multiple ports for use with multiple web severs on the same system.
date: Sep 3 2011
permalink: /blog/wordpress/wordpress-on-multiple-ports.html
---

So this is a little tidbit I thought I would share as I came across the question quite a few times trying to find an answer to it.  Basically I'm testing Nginx and Apache side by side on the same server with Apache on port 80 and Nginx on port 81.  The problem is I want to use the same WordPress install for both servers with different ports.

The only way to change a port in WordPress is to specify the `siteurl` and `home` url options under the Settings tab in the admin.  This meant we would have to do a little hacking.

Fortunately there is one central function that handles pulling of options from the database.  All we have to do is add one line in there to check if we are running on port 81 and hard code our value in there.  Yes hard coding is not the cleanest way to do it, but technically are url is hard coded in the db anyway.

So lets open up the functions.php file from WordPress.

~~~
$ vi /path/to/wordpress/wp-includes/functions.php
~~~

Inside this file you will see some code like so:

~~~
function get_option( $option, $default = false ) {
  ....

  return apply_filters( 'option_' . $option, maybe_unserialize( $value ) );
}
~~~

All we need to do is modify the value before the return by checking if the option is either `home` or `siteurl` and on the alternate port we are running WordPress on, which in this case is port 81.

~~~
function get_option( $option, $default = false ) {
  ....

  if(($option == 'siteurl' || $option == 'home') &amp;&amp; $_SERVER['SERVER_PORT'] == '81')
    $value = 'http://www.duchnik.com:81/tutorials';

  return apply_filters( 'option_' . $option, maybe_unserialize( $value ) );
}
~~~

We can go on adding multiple conditional statements here for other ports as well if we liked.  Also I could get a little fancier here by parsing the original `siteurl` and `home` url options already in the db, then use that to reconstruct the url with the new port, but didn't find that necessary for the time being.