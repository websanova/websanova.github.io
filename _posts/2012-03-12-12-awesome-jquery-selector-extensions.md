---
layout: master
title: 12 Awesome jQuery Selector Extensions
keywords: jquery, selector, extension
description: List of 12 jQuery selectors for extending jQuery.
date: Mar 12 2012
permalink: /blog/jquery/12-awesome-jquery-selector-extensions
redirect_from: /tutorials/jquery/12-awesome-jquery-selector-extensions.html
---

I recently began writing my own jQuery selectors more and more as I realized it's a very nice and clean way to accomplish some specific tasks that I may normally have used an iterator for.  It's one of those things that if you're not looking for you'll never see, so I decided to write a little tutorial on it as setting up your own selectors is extremely easy with jQuery.

Here is a little boiler plate for creating your own jQuery selectors and pretty much all you'll need:

~~~
$.extend($.expr[':'],
{
    selectorName: function(el, i, m)
    {
        return true/false;
    },
    
    selectorName2: function(el, i, m)
    {
        return true/false;
    }
});
~~~
Below are two ways to call a selector, one with an argument and one without.

~~~
$("#container :selectorName");
$("#conainert :selectorName(#element)");
$("#conainert :selectorName(>300)");
~~~

The "i" and "m" paramaters you won't always need, they are just the current index of the element since it passes in a collection and the match for an argument.  The matcher comes in when you are passing an argument, it's a regular expression matcher and returns something like this:

~~~
[":width(>100)", "width", "", ">100"]
~~~

You will mostly be using the m[3] value, from there it's up to you to do whatever you like with the value you pass in.  I've given a bunch of examples below to give you a better idea of the range and flexibility you can go with creating your own selectors.  I basically just keep a little selectors.js file handy and toss whatever I need for the current project in there.

Also note that we can chain all these selectors together along with the existing jQuery selectors which is where it gets really interesting.

~~~
$("#container :isBold:even");
$("#container :leftOf(#element):width(>100):height(>100)");
~~~

You can download the [library and checkout documentation for it](https://github.com/websanova/wExtensions).

Here is a list of 12 custom selector samples I have created and used in my own projects.  You can pretty much put anything in these selectors as long as you return a true or false value based on whether the current element passes the selector test.

### :loaded

Select all loaded images.

~~~
$.extend($.expr[':'],
{
    loaded: function(el)
    {
        return $(el).data('loaded');
    }
}

$('img').load(function(){ $(this).data('loaded', true); });
$('img:loaded');
~~~

### :width

Select all elements with width greater/less than the specified value.

~~~
$.extend($.expr[':'],
{
    width: function(el, i, m)
    {
        if(!m[3]||!(/^(<|>)\d+$/).test(m[3])) {return false;}
        return m[3].substr(0,1) === '>' ? 
            $(el).width() > m[3].substr(1) : $(el).width() < m[3].substr(1);
    }
}

$('#container :width(>100)');
~~~

### :height

Select all elements with height greater/less than the specified value.

~~~
$.extend($.expr[':'],
{
    height: function(el, i, m)
    {
        if(!m[3]||!(/^(<|>)\d+$/).test(m[3])) {return false;}
        return m[3].substr(0,1) === '>' ? 
            $(el).height() > m[3].substr(1) : $(el).height() < m[3].substr(1);
    }
}

$('#container :height(<100)');
~~~

### :leftOf

Select all elements left of the specified element.

~~~
$.extend($.expr[':'],
{
    leftOf: function(el, i, m)
    {
        var oe = $(el).offset();
        var om = $(m[3]).offset();

        return oe.left + $(el).width() < om.left;
    }
}

$('#container :leftOf(#element)');
~~~

### :rightOf

Select all elements right of the specified element.

~~~
$.extend($.expr[':'],
{
    rightOf: function(el, i, m)
    {
        var oe = $(el).offset();
        var om = $(m[3]).offset();

        return oe.left > om.left + $(m[3]).width();
    }
}

$('#container :rightOf(#element)');
~~~

### :external

Select all anchor tags with links to domains other than the current domain.

~~~
$.extend($.expr[':'],
{
    external: function(el)
    {
        if(!el.href) {return false;}
        return el.hostname && el.hostname !== window.location.hostname;
    }
}

$('#container :external');
~~~

### :target

Select all anchor tags with the specified target.

~~~
$.extend($.expr[':'],
{
    target: function(el, i, m)
    {
        if(!m[3]) {return false;}
        return (m[3] === '_self' && ($(el).attr('target') == '' || !el.target)) || 
            (m[3] === $(el).attr('target'));
    }
}

$('#container :target(_self)');
~~~

### :inView

Select all elements that have any part in the viewable window.

~~~
$.extend($.expr[':'],
{
    inView: function(el)
    {
        var offset = $(el).offset();

        return !(
            (offset.top > $(window).height() + $(document).scrollTop()) || 
            (offset.top + $(el).height() < $(document).scrollTop()) || 
            (offset.left > $(window).width() + $(document).scrollLeft()) || 
            (offset.left + $(el).width() < $(document).scrollLeft())
        )
    }
}

$('#container :inView');
~~~

### :largerThan

Select all elements larger than the specified one.

~~~
$.extend($.expr[':'],
{
    largerThan: function(el, i, m)
    {
        if(!m[3]) {return false;}
        return $(el).width() * $(el).height() > $(m[3]).width() * $(m[3]).height();
    }
}

$('#container :largerThan(#element)');
~~~

### :isBold

Select all elements that have a font-weight of 700.

~~~
$.extend($.expr[':'],
{
    isBold: function(el)
    {
        return $(el).css("fontWeight") === '700';
    }
}

$('#container :isBold');
~~~

### :color

Select elements with specified color in rgb format.

~~~
$.extend($.expr[':'],
{
    color: function(el, i, m)
    {
        if(!m[3]) {return false;}
        return $(el).css('color') === m[3];
    }
}

$("#container :color(rgb(255, 0, 0))");
~~~

### :hasId

Select elements that have an id attribute specified.

~~~
$.extend($.expr[':'],
{
    hasId: function(el)
    {
        return $(el).attr('id') !== undefined && $(el).attr('id') !== '';
    }
}

$("#container :hasId");
~~~

I find building your own custom selectors is something you won't use often but is definitely a great tool to be aware of and have in your arsenal.  They can come in very handy in some situations and help to keep your code cleaner and more easily maintained.