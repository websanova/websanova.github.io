---
layout: master
title: Installing Node.js with Nginx Proxy
keywords: nodejs, node, nginx, proxy, windows, ubuntu
description: Node.js is great for serving app. But it becomes cumbersome to deal with static files. It's much better to let an existing server such as Nginx handle this which it already does so well.
date: Mar 08 2014
permalink: /blog/nginx/installing-node-js-with-nginx-proxy.html
---

Setting up a node.js server is very easy and only takes about a minute or two to get a basic "Hello World" example going. The thing is your `Node.js` instance only runs one app so you will need multiple instances on multiple ports to run separate apps. Chances are you will probably want to run these on port `80` like a standard web app so this requires the use of a web server like Nginx to act as a proxy between requests.

## Install Node.js

Our first step is to install node.js which we can do by downloading and running the install from the [nodejs.org](http://nodejs.org/download/) site. Once installed we should have available on our command line the `node` command.

~~~
> node -v
> v0.10.22
~~~

## Create a Node App

From here we can create a JavaScript file absolutely anywhere on our system and run our node server. So for example let’s create a `server.js` file with the following contents:

~~~
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(3000, '127.0.0.1');
console.log('Server running at http://127.0.0.1:3000/');
~~~

Then run node:

~~~
> node server.js
> Server running at http://127.0.0.1:3000/
~~~

We can navigate to `http://127.0.0.1:3000` in our browser and we will see our “Hello World” displayed.

To stop our `node.js` we can just hit `ctrl+c` to stop the process.

The thing is we probably don’t want to run our app from port `3000` but rather on port `80`. We could in theory setup our Node server to run on port 80 but since this is such a widely used port and would interfere with any other services running on port 80 we will leave it at port 3000 and instead setup a web proxy using Nginx.

## Installing Nginx

Nginx is just as easy if not easier than installing node.js. All we need to do is download and extract the contents from the [nginx.org](http://nginx.org/en/download.html) website somewhere on to our system. If we then navigate to our nginx folder we can test it out.

~~~
> nginx -v
> nginx version: nginx/1.4.6
~~~

From here we can run the `nginx` executable, open up our browser at `http://localhost` and we will see an Nginx "Welcome" page.

If you’re using Windows we’ll want to toss the `nginx` executable into `PATH` environment variable to make it available globally.

To stop and reload nginx:

~~~
> nginx -s stop
> nginx -s reload
~~~

Note we may need to keep a separate terminal open to run these commands as we can not break the Nginx process using `ctrl+c`.

## Configure Nginx

The easiest configuration for Nginx is to just open up the `conf/nginx.conf` file and add entries in there. To start we will create an entry for longer domain names to avoid the `bit bucket` errors. Add this within the `http` section.

~~~
http {
  server_names_hash_bucket_size 64;
  ...
}
~~~

Then we will create an entry to proxy requests for our domain to our node app. As an example lets say we create a website on the domain `http://robstodo.com` that proxies requests at port 80 to our node app serving requests from port 3000. Below is what our configuration would look like:

~~~
http {
  ...

  upstream app_robstodo {
    server 127.0.0.1:3000;
  }

  server {
    listen 80;
    server_name www.robstodo.com robstodo.com;
    access_log /path/to/logs/nginx/minitorials.log;

    location / {
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_set_header X-NginX-Proxy true;

      proxy_pass http://app_robstodo/;
      proxy_redirect off;
    }
  }
}
~~~

Nothing too much going on in the config. As we can see it will listen on port 80 for `robstodo.com` and `www.robstodo.com`. The `app_robstodo` is just a reference to tell it to proxy requests to our node app sitting at `localhost:3000`.

You should now be able to re/start your Nginx server then navigate to both `http://robstodo.com` and see the “Hello World” message in your browser.

You can also check out this Stackoverflow tutorial on setting up [Node.js with Nginx on Ubuntu](http://stackoverflow.com/questions/5009324/node-js-nginx-and-now).