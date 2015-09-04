---
layout: master
title: Serving pages dynamically with Node.js
keywords: nodejs, node, http, file, html, server, mime
description: Setting up a basic HTTP server in Node.js is as simple as copy and pasting five lines of code. However if we will want to create something more appropriate for websites and handle our pages more dynamically.
date: Mar 10 2014
img: /wp-content/uploads/2014/03/serving-pages-dynamically-with-nodejs.png
permalink: /blog/node-js/serving-pages-dynamically-with-node-js.html
---

Setting up a basic HTTP server in `Node.js` is as simple as copy and pasting five lines of code and running `node file.js` as you can see in my [previous tutorial on installing Node.js](/blog/nginx/installing-node-js-with-nginx-proxy). However, we will probably want to setup something a little more dynamic pretty soon to start serving requests based on a URL pattern.

Let’s start with a simple example to see how our Node server behaves by outputting the request URL into our console.

~~~
var http = require('http');

http.createServer(function (req, res) {
  console.log(req.url);
  res.end();
})
.listen(3000, '127.0.0.1');
~~~

If we start this Node instance and navigate to `http://127.0.0.1:3000` in our browser we will actually see two requests.

~~~
/
/favicon.ico
~~~

Note that both these requests are triggered by the browser separately. The first for the resource at `/` and the second for the `/favicon.ico`.

At the most basic level we can serve a single file by reading and outputting it’s contents. So for instance if we wanted to serve an HTML file:

~~~
var fs = require('fs'),
    http = require('http');

http.createServer(function (req, res) {
  fs.readFile('./index.html', function (err, html) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write(html + '');  
    res.end();
  });
})
.listen(3000, '127.0.0.1');
~~~

This is great but we probably want to be able to serve files based on what the URL is. Below we will make things a little more dynamic by serving the requested resource with `index.html` as a default if no file is specified. We will also check if the file exists and serve a `404` page if no resource exists.

~~~
var fs = require('fs'),
    http = require('http');

http.createServer(function (req, res) {
  var path = '.' + req.url;

  // Default to index.html if no file is specified.
  if (req.url.split('.').length === 1) {
    path += (req.url[req.url.length - 1] !== '/' ? '/' : '') + 'index.html';
  }

  // Check if resource exists and serve 404 page if it doesn't.
  fs.exists(path, function (exists) {
    if ( ! exists) {
      path = './views/404.html';
    }
    
    fs.readFile(path, function (err, html) {
      res.writeHead(200, {'Content-Type': 'text/html'});
      res.write(html + '');  
      res.end();
    });
  });
})
.listen(3000, '127.0.0.1');
~~~

There we have it, a basic Node.js server that will serve static HTML files. You’ve probably noticed that our `Content-Type` is set to `text/html`. If we were to request a resource like an image you will see it being served with the HTML MIME type we specified coming out as raw data and looking like a bunch of giberish.

From here we will want to start being able to server other types of files other than HTML like our `favicon.ico` file and images. We’ll cover this in the next tutorial on handling MIME types with `Node.js`.