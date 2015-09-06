---
layout: master
title: Websanova url.js v2.0.0 for Web and Node.js Released
keywords: websanova, jquery, javascript, nodejs, node, js, url, wurl, parser, released
description: The websanova url.js parser has released version 2.0.0. The new version now features the much anticipated tld and sub domain support.
date: Sep 04 2015
img: /img/logo-200x200.png
permalink: /blog/javascript/url-node-js-2-released.html
---

It's been a while so we finally decided to rewrite an old goody to add some more features. The new version of url.js comes for both web and Node.js. It comes with some new features including proper `tld` and `sub` domain support. Also support for array parameters. Even with all the new features we managed to make the file size even smaller.

* [Documentation](https://github.com/websanova/js-url#url)
* [Issues](https://github.com/websanova/js-url/issues)
* [Download](https://github.com/websanova/js-url/releases)

List of changes here:

* Support for tld, domain and sub domain.
* Comes in two versions `url-tld.min.js` and `url.min.js`.
* Support for arrays in query and hash parameters.
* Support for mailto: protocol.
* Passing `url('[]')` now returns entire parsed object for url.
* Passing `url('?')` now returns all query parameters as an object.
* To get entire query string use `url('query')`
* Passing `url('#')` now returns all hash parameters as an object.
* To get entire hash string use `url('hash')`
* The `tld`, `domain` and `sub` arguments are all disabled for the non tld version.
* For tld version `tld`, `domain` and `sub` now return appropriate values (if tld matches).
* You can now use `url('/1')` or just `url(1)` for fetching path parts.
* Passing `url('/')` will return all path parts as an array.
* Passing `url('.')` will return all domain parts as an array.
* If no matches found returns undefined now in most cases.
* Smaller file size, more efficient, simpler code.

As always, we hope you enjoy.