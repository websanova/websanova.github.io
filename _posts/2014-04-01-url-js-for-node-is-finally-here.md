---
layout: master
title: Url.js for Node is Finally Here
keywords: nodejs, node, url, javascript, package
description: After the popularity of the websanova url.js package we got a lot of requests for a node.js version. Wait no longer, it's finally here.
date: Apr 01 2014
permalink: /blog/node-js/url-js-for-node-is-finally-here.html
---

You asked for it, so here it is. A few of the valiant readers of Websanova have been asking me for a Node version of the tiny but popular [url.js](http://github.com/websanova/js-url) library I had written some time ago. A few attempts were made to use the existing library into Node but it seems it’s not that straight forward. As I’ve been learning a lot of Node lately I have finally ported it over and you can find it on both GitHub and the official NPM directory at the links below.

* [url.js for Node on GitHub](http://github.com/websanova/node-url)
* [url.js for Node on NPM](https://www.npmjs.org/package/wurl)

Unfortunately there was already a package named `url` so I had to call it something else. I ended up going with `wurl` but you can still attach it to any variable name you like.

You can include it in your project by running:

~~~
npm install wurl
~~~

Don’t forget web JavaScript and jQuery versions of the URL parser are also available on the [Websanova GitHub page](https://github.com/websanova/js-url).
