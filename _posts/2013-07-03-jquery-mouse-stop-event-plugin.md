---
layout: master
title: jQuery .mousestop() Event Plugin
keywords: javascript, jquery, mouse, event, mousestop, custom, websanova
description: It’s not very often we think about triggering a stop event over an element. But perhaps that is because the event has never existed in JavaScript.
date: July 03 2013
permalink: /blog/jquery/jquery-mouse-stop-event-plugin.html
---

It’s not very often we think about triggering a stop event over an element. But perhaps that is because the event has never existed in JavaScript. Actually the first time I came across ever needing a `mousestop` event was when I was building my [jQuery Tooltip Plugin](http://wtooltip.websanova.com). I was attempting to closely mimic the browsers default behavior and noticed that the `title` tooltip that appears on elements only comes after the mouse stops moving.

I wanted to create this for my tooltip as well so I ended up writing this simple little `mousestop` event plugin for jQuery. It takes advantage of the [jQuery Special Events API](/custom-events-using-the-jquery-special-events-api) which allows you to add events right into the fabric of jQuery. This means we can bind the events using `on` and `bind` as well as trigger it using `trigger` or calling `mousestop` on an element.

* [mousestop Demo](http://mousestop.websanova.com)
* [mousestop Download](https://github.com/websanova/mousestop/tags)
* [mousestop Documentation](https://github.com/websanova/mousestop#mousestopjs)
* [mousestop Issues](https://github.com/websanova/mousestop/issues)

This little event plugin is only ~1Kb minified and works just like all the other mouse events in jQuery allowing you to method chain them to an element and set a callback for the event. Check out the documentation for more info on usage.