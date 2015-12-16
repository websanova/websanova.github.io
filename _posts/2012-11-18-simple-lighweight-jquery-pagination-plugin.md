---
layout: master
title: Simple, Lighweight jQuery Pagination Plugin
keywords: jquery, javascript, plugin, websanova, wpaginate, paginate, pagination
description: A simple lightweight jQuery pagination plugin. Comes with ajax support allowing you to customize for your needs.
date: Nov 18 2012
permalink: /blog/jquery/simple-lighweight-jquery-pagination-plugin.html
redirect_from:
  - /tutorials/jquery/simple-lighweight-jquery-pagination-plugin.html
  - /blog/uncategorized/simple-lighweight-jquery-pagination-plugin.html
---

After some frustration trying some jQuery pagination plugins I found on Google, many of which were written using jQuery 1.3 or 1.4, I decided to write a new one as it seems one was needed. This pagination plugin is very lightweight and flexible and has ajax support allowing you to customize it for your own needs. It comes pretty standard out of the box with what you would expect. There are four basic parameters for setting `total`, `index`, `limit` and the `url`.

* [wPaginate Demo](http://wpaginate.websanova.com/)
* [wPaginate Download](https://github.com/websanova/wPaginate/tags)
* [wPaginate Documentation](https://github.com/websanova/wPaginate#wpaginatejs)
* [wPaginate Issues](https://github.com/websanova/wPaginate/issues)

## Static URL

In this case the indexes would just be appended to the url so that you would have paths like /some/path/0, /some/path/20 and so on.

~~~
$('#elem').wPaginate({
    total: 453,
    index: 200,
    limit: 20,
    url: '/some/path'
});
~~~

## Dynamic URL

If we want a little more flexibility we can set the url dynamically using a function.

~~~
$('#elem').wPaginate({
    total: 453,
    index: 200,
    limit: 20,
    url: function(i){ return '/some/' + i + '/path' }
});
~~~

## Ajax

Finally we can also set it up in ajax mode allowing us to pull pages via ajax and set them without reloading the page. This can be done very simply by setting the `ajax` setting to `true`. When this is done the `url` setting becomes a callback that you can use to do whatever you like to load your ajax page.

~~~
$('#elem').wPaginate({
    total: 453,
    index: 200,
    limit: 20,
    ajax: true,
    url: function(i){ /* load and set page here... */ }
});
~~~

## Themes & Styling

There are four themes out of the box, but it is very easy to style your own. The basic CSS for a theme would look something like this:

~~~
._wPaginate_black ._wPaginate_link{border-color:#000; color:#000;}
._wPaginate_black ._wPaginate_link:hover,
._wPaginate_black ._wPaginate_link_active{border-color:#FF0000; color:#FF0000;}
~~~

Each element has it’s own class tag called `_wPaginate_link` with key words for the navigation items `_wPaginate_first`, `_wPaginate_prev`, `_wPaginate_next` and `_wPaginate_last`. From there we have pretty much all the flexibility we need to produce a look to match our app.