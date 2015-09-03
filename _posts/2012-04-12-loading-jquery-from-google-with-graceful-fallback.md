---
layout: master
title: Loading jQuery From Google With Graceful Fallback
keywords: jquery, load, google, graceful, fallback
description: Short article with code sample describing how to load jQuery from Google with a gracefuly local fallback.
date: Apr 12 2012
permalink: /blog/jquery/loading-jquery-from-google-with-graceful-fallback
redirect_from: /tutorials/jquery/loading-jquery-from-google-with-graceful-fallback.html
---

This is a little example of how to load jQuery from Googles CDN. I prefer to load it from there as it's nicely minified and g-zipped as well there is a better chance a user would have already picked up this file from another site so it will already be cached. This can improve the loading time of your site and every little bit helps. We also provide a fallback in case Google decides to crash for whatever reason.

~~~
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
<script>
    window.jQuery || document.write('&amp;lt;script src="js/libs/jquery-1.7.1.min.js"><\/script>');
</script>
~~~

It's pretty straightforward, basically we load from Google first if we can. Then we check if the jQuery object exists, if not we load our local version and voila.