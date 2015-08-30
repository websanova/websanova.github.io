---
layout: default
title: Custom Events Using the jQuery Special Events API
keywords: javascript, jquery, events, api, special
description: It’s not often we need our own jQuery events, but when we do it’s nice to know we can create an elegant solution that behaves like any other event would.
date: July 15 2013
permalink: /blog/jquery/custom-events-using-the-jquery-special-events-api.html
---

It’s not often we need our own jQuery events, but when we do it’s nice to know we can create an elegant solution that behaves like any other event would. We want to be able to add an event right into the fabric of jQuery that we can bind using the `bind` and `on` methods.

The jQuery special events API is not something that you will come across normally and in fact is not even documented in the jQuery docs online. I only came about it recently after coming across this great article on [jQuery Special Events](http://benalman.com/news/2010/03/jquery-special-events) which goes into a lot of detail on how the API works and provides a lot of great examples. I ended up updating my [mousestop](http://mousestop.websanova.com) plugin which was a perfect candidate for using the special events API.

In this article I just want to briefly go over the events API to help get you started and give you an idea of how it works. But definitely read the above mentioned article for more information if you plan on writing any “special” events.

Normally the easy way to create a custom event is to just use the `bind` method and pass it a custom name and then use the `trigger` method to trigger it. For example we might set up some kind of collapsible menu like so:

~~~
$("#tree").bind("collapse", function(e) {
    $(e.target).children().slideUp().end().addClass("collapsed");
}).bind("expand", function(evt) {
    $(e.target).children().slideDown().end().removeClass("collapsed");
})).toggle(function() { // toggle between
    $(this).trigger("collapse");
}, function() {
    $(this).trigger("expand");
});
~~~

In the above example we bind the events, `collapse` and `expand` and trigger them using the `toggle` function. This is not a bad solution at all, although it does take a bit of code to setup. We need to always bind to the elements and then trigger on those elements. It would be much nicer to have some kind of `toggleMenu` event instead that we could just attach and call directly like so:

~~~
$("#tree").toggleMenu(function(){ /* do something */ });
~~~

This is quite a simple example but what if our our event comprises of multiple events or actions working together. Also what if we want some default options and the ability to override those options. Our code will start getting bigger and messier not too mention not very reusable at all.

Let’s look at the [mousestop](http://mousestop.websanova.com) plugin I recently updated using the jQuery events API. This is a small event plugin I wrote for my [wTooltip](http://wtooltip.websanova.com/) plugin that needed to detect a `mousestop` in order to trigger the tooltip. We’ll go over each piece separately briefly following the code sample.

~~~
(function($) {
    $.event.special.mousestop = {
        setup: function(data) {
            $(this).data('mousestop', _data(data))
                   .bind('mouseenter.mousestop', _mouseenter)
                   .bind('mouseleave.mousestop', _mouseleave)
                   .bind('mousemove.mousestop', _mousemove);
        },
        teardown: function() {
            $(this).removeData('mousestop')
                   .unbind('.mousestop');
        }
    };

    function _mouseenter() {
        var _self = this,
            data = $(this).data('mousestop');

        this.movement = true;

        if(data.timeToStop) {
            this.timeToStopTimer = window.setTimeout(function() {
                _self.movement = false;
                window.clearTimeout(_self.timer);
            }, data.timeToStop);
        }
    }

    function _mouseleave() {
        window.clearTimeout(this.timer);
        window.clearTimeout(this.timeToStopTimer);
    }
    
    function _mousemove() {
        var $el = $(this),
            data = $el.data('mousestop');

        if(this.movement) {
            window.clearTimeout(this.timer);
            this.timer = window.setTimeout(function() {
                $el.trigger('mousestop');
            }, data.delay);
        }
    }

    function _data(data) {
        if($.isNumeric(data)) {
            data = {delay: data};
        }
        else if(typeof data !== 'object') {
            data = {};
        }

        return $.extend({}, $.fn.mousestop.defaults, data);
    }

    $.fn.mousestop = function(data, fn) {
        if (typeof data === 'function') { fn = data; }
        return arguments.length > 0 ? 
            this.bind('mousestop', _data(data), fn) :
            this.trigger('mousestop');
    };

    $.fn.mousestop.defaults = {
        delay: 300,
        timeToStop: null
    };
})(jQuery);
~~~

## Mousestop

The `mousestop` is actually a small amount of code. Without the added `timeToStop` option all we really need is the `mouseleave` call to clear the `timeout` and the `mousemove` function to constantly reset the timer to detect a new stop.

## Setup & Teardown

There are other functions available here including `add`, `remove` and `_default` but for the most part we’re interested in the `setup` and `teardown` functions. They let us setup what happens during any binding and unbinding functions like `on` or `bind` and `unbind`. In this example we just setup handlers for some mouse events on the element.

## Triggering

It’s important to note that the event will be triggered in different ways. When using the `on` or `bind` methods the functions will go straight to the events API and call the `setup` method. Realize that the `$.fn.mousestop` function is simply a convenience method so that you can call the function using `$('#el').mousestop();` rather then using the `trigger` method all the time.

## Data

You will notice there is a private `_data` function that is called from both the `$.fn.mousestop` and `setup` functions that lets us centralize data formatting from both spots. We also have the `$.fn.mousestop.defaults` object where users can set their own defaults globally.

And that’s pretty much the basics. There is really not that much difference from writing some regular functions other than it comes out much cleaner and works with all the other jQuery event methods for binding. The idea is to make our code more modular and reusable, that extend jQuery in ways we would expect rather than building out code on top of code.