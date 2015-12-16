---
layout: master
title: Nginx Catch All Rewrite
keywords: nginx, catchall, rewrite
description: Article covering a how to setup a catch all rewrite in Nginx.
date: Sep 11 2011
permalink: /blog/nginx/nginx-catch-all-rewrite.html
redirect_from:
  - /nginx-catch-all-rewrite.html
---

As I'm playing more with Nginx, I'm finding I have to convert a lot of my common Apache configuration over to Nginx.  Here is one that is frequently used, it's a catch all rewrite that takes any request that does not exist as a file or directory and rewrites it to the index.

### The Apache way

~~~
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
~~~

### The Nginx Way

~~~
if (!-e $request_filename)
{
        rewrite ^/(.*)$ /index.php?/$1 last;
        break;
}
~~~

In Nginx we would put this is our server directive, probably just above any location directives.  The whole thing might looks something like this:

~~~
server {
        listen          81;
        listen          duchnik.com:81;
        server_name     duchnik.com www.duchnik.com;
        root            /home/webadmin/www.duchnik.com/htdocs;
        index           index.html index.htm index.php;

        if (!-e $request_filename)
        {
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