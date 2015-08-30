---
layout: master
title: Getting Wordpress to Work With Nginx
keywords: wordpress, nginx
description: Article describing getting Wordpress to work with Nginx web server.
date: Sep 16 2011
permalink: /blog/nginx/getting-wordpress-to-work-with-nginx.html
---

Since Nginx does not use `.htaccess` files we need to convert all our `.htaccess` rules to Nginx.  If you seen my previous tutorial on [Nginx Catch All Rewrite](/nginx-catch-all-rewrite) then you will see the similarities here.  

We basically just need to rewrite all non file/directory requests to `index.php`.  If your WordPress site is set up at the root, meaning its at http://www.duchnik.com for example, then all you will simply need is this:

~~~
if (!-e $request_filename)
{
        rewrite ^/(.*)$ /index.php?/$1 last;
        break;
}
~~~

If you have your blog setup in a separate directory like i do, so lets say `http://websanova.com/tutorials` then you will need to specify that directory.

~~~
if (!-e $request_filename)
{
        rewrite ^/tutorials/(.*)$ /tutorials/index.php?/$1 last;
        break;
}
~~~

You can also just combine the two, which is what I do, allowing me to run my framework and my blog separately.

~~~
if (!-e $request_filename)
{
        rewrite ^/tutorials/(.*)$ /tutorials/index.php?/$1 last;
        rewrite ^/(.*)$ /index.php?/$1 last;
        break;
}
~~~

In the end you will get something that looks like this:

~~~
server {
        listen               81;
        listen               duchnik.com:81;
        server_name          duchnik.com www.duchnik.com;
        root                 /home/webadmin/www.duchnik.com/htdocs;
        index                index.html index.htm index.php;

        if (!-e $request_filename)
        {
                rewrite ^/tutorials/(.*)$ /tutorials/index.php?/$1 last;
                rewrite ^/(.*)$ /index.php?/$1 last;
                break;
        }

        location ~ \.php$ {
                root /home/webadmin/www.duchnik.com/htdocs;

                fastcgi_pass 127.0.0.1:9000;
                fastcgi_index index.php;
                include /etc/nginx/fastcgi.conf;
        }
}
~~~