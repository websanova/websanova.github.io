---
layout: master
title: JavaScript URL Parser
keywords: javascript, url, parser
description: Sample code and demo of the Websanova JavaScript url parser.
date: Oct 27 2012
permalink: /blog/javascript/javascript-url-parser.html
redirect_from: "/tutorials/javascript/javascript-url-parser.html"
---

This is a nice little library I wrote for myself as I found I was quite frequently parsing a url in my projects.  I was constantly building little custom functions but decided to finally build out a nice simple and lightweight library to help parse url's with.  It comes in at only ~1.6 Kb minified and ~0.6Kb gzipped.  You can find the latest version of the [url() JavaScript URL parser on GitHub](https://github.com/websanova/js-url#url) which contains a minified and jQuery version.

~~~
http://rob:abcd1234@www.domain.com/path/index.html?query1=test&amp;silly=willy#test=hash&amp;chucky=cheese
~~~

~~~
url();            // http://rob:abcd1234@www.domain.com/path/index.html?query1=test&amp;silly=willy#test=hash&amp;chucky=cheese
url('hostname');  // www.domain.com
url('domain');    // domain.com
url('tld');       // com
url('sub');       // www
url('.0')         // (an empty string)
url('.1')         // www
url('.2')         // domain
url('.-1')        // com
url('auth')       // rob:abcd1234
url('user')       // rob
url('pass')       // abcd1234
url('port');      // 80
url('protocol');  // http
url('path');      // /path/index.html
url('file');      // index.html
url('filename');  // index
url('fileext');   // html
url('1');         // path
url('2');         // index.html
url('3');         // (an empty string)
url('-1');        // index.html
url(1);           // path
url(2);           // index.html
url(-1);          // index.html
url('?');         // query1=test&amp;silly=willy
url('?silly');    // willy
url('?poo');      // (an empty string)
url('#');         // test=hash&amp;chucky=cheese
url('#chucky');   // cheese
url('#poo');      // (an empty string)
~~~

We can also pass a url in and call all the same options on it:

~~~
url('sub', 'test.www.domain.com/path/here');               // test.www
url('protocol', 'www.domain.com/path/here');               // http
url('path', 'http://www.domain.com:8080/some/path');       // /some/path
url('port', 'http://www.domain.com:8080/some/path');       // 8080
url('protocol', 'https://www.domain.com:8080/some/path');  // https
etc...
~~~