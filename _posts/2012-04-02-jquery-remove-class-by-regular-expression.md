---
layout: master
title: jQuery Remove Class by Regular Expression
keywords: jquery, remove, class, regular, expression, regex
description: Sample code for removing a class or classes via regular expression through a jQuery utility extension.
date: Apr 2 2012
permalink: /blog/jquery/jquery-remove-class-by-regular-expression.html
redirect_from: /tutorials/jquery/jquery-remove-class-by-regular-expression.html
---

I recently came across a problem in an app I was working on of needing to remove a class name from an element that began with a particular string.  The removeClass() method that comes with jQuery does not seem to be able to handle any type of wildcards and only works on an exact match.  This led me to write my own little removeClassRegEx() and hasClassRegex() methods to be able to handle this problem which I have shared below.

I find these regex methods come in handy when you need to toggle a set of styles that all follow a certain naming convention, for instance a color theme or layout structure.  In my particular app the user has the ability to pick themes from a menu which would update the page on the fly.  It's useful anytime you want to toggle multiple elements on a page by only updating the class name in a parent div and have all the child elements follow through based on that parent class name.  Take the following examples:

~~~
<div class="layout_theme_blue layout_view_detailed">
    <!-- content -->
</div>

<div class="layout_theme_red layout_view_simple">
    <!-- content -->
</div>
~~~

Normally we would have to go in and remove all the classes one by one before adding a new one.  More importantly if we decided to add a new theme we would have to update this code every time. 

~~~
function update_theme(name)
{
    $('#container')
    .removeClass('layout_theme_blue layout_theme_red etc...')
    .addClass('layout_theme_' + name);
}
~~~

It would be much better to use a regular expression pattern to match and remove any instance of "layout_theme_".  Now we can continue to add themes without needing to visit this code again.

~~~
function update_theme(name)
{
    $('#container')
    .removeClassRegEx(/^layout_theme_/)
    .addClass('layout_theme_' + name);
}
~~~

Another way to do this could be something like below, however I do not find this a very elegant solution and it doesn't give us much reusability.  The above code is much more clean, concise and easier to read.

~~~
function update_theme(name)
{
    var classes = $("#container").attr('class').replace(/layout_theme_[a-zA-Z0-9_]*/g, '');
    $("container").attr('class', classes).addClass('layout_theme_' + name);
}
~~~

### .removeClassRegEx()
The code is quite simple,  all it does is match the pattern against each class name individually and if a match is found it is removed.  This gives us a ton of flexibility in a very small footprint with only a few lines of code.

~~~
(function($)
{
    return this.each(function()
    {
        var classes = $(this).attr('class');

        if(!classes || !regex) return false;

        var classArray = [];
        classes = classes.split(' ');

        for(var i=0, len=classes.length; i<len; i++) if(!classes[i].match(regex)) classArray.push(classes[i]);

        $(this).attr('class', classArray.join(' '));
    });
})(jQuery);
~~~

~~~
<span id="remClassRegEx" class="Test Testing someTest aaTestaa leaveme"></span>
    
$("#remClassRegEx").removeClassRegEx();         // Test Testing someTest aaTestaa leaveme
$("#remClassRegEx").removeClassRegEx('');       // Test Testing someTest aaTestaa leaveme
$("#remClassRegEx").removeClassRegEx('Test');   // leaveme
$("#remClassRegEx").removeClassRegEx(/ /);      // Test Testing someTest aaTestaa leaveme
$("#remClassRegEx").removeClassRegEx(/Test/);   // leaveme
$("#remClassRegEx").removeClassRegEx(/^Test/);  // someTest aaTestaa leaveme
$("#remClassRegEx").removeClassRegEx(/Test$/);  // Testing aaTestaa leaveme
$("#remClassRegEx").removeClassRegEx(/^Test$/); // Testing someTest aaTestaa leaveme
$("#remClassRegEx").removeClassRegEx(/test/);   // Test Testing someTest aaTestaa leaveme
$("#remClassRegEx").removeClassRegEx(/test/i);  // leaveme
~~~

### .hasClassRegEx()
We can also check if a certain class exists using a regular expression pattern matcher. 

~~~
(function($)
{
    $.fn.hasClassRegEx = function(regex)
    {
        var classes = $(this).attr('class');
        
        if(!classes || !regex) return false;
        
        classes = classes.split(' ');
        
        for(var i=0, len=classes.length; i&amp;lt;len; i++)
            if(classes[i].match(regex)) return true;
        
        return false;
    }; 
})(jQuery);
~~~

~~~
<span id="hasClassRegEx" class="Test Testing someTest aaTestaa"></span>
    
$("#hasClassRegEx").hasClassRegEx();         // false
$("#hasClassRegEx").hasClassRegEx('');       // false
$("#hasClassRegEx").hasClassRegEx('Test');   // true
$("#hasClassRegEx").hasClassRegEx(/ /);      // false
$("#hasClassRegEx").hasClassRegEx(/Test/);   // true
$("#hasClassRegEx").hasClassRegEx(/^Test/);  // true
$("#hasClassRegEx").hasClassRegEx(/Test$/);  // true
$("#hasClassRegEx").hasClassRegEx(/^Test$/); // true
$("#hasClassRegEx").hasClassRegEx(/test/);   // false
$("#hasClassRegEx").hasClassRegEx(/test/i);  // true
~~~

Sometimes we can get constrained by the tools we use not letting us think about alternate ways to solve a problem.  I find these little methods are great to have handy and can really create some elegant solutions.  I hope you can find it as useful as I have.