---
layout: master
title: jQuery .cssAll() – Retrieving Multiple CSS Properties
keywords: jquery, plugin, javascript, css, cssall
description: The current implementation of the .css() method in jQuery allows us to set multiple properties to an element, but only allows us to pull one property at a time.
date: Nov 06 2012
permalink: /blog/jquery/jquery-cssall-retrieving-multiple-css-properties.html
---

The current implementation of the `.css()` method in jQuery allows us to set multiple properties to an element, but only allows us to pull one property at a time. This is a little function I wrote for myself as I found myself sometimes wanting to pull more than one CSS property from an element at a time to assign to one or more other elements. It returns an object that can then easily be passed into another element using the `.css()` method.

We can pull all the values by simply calling the `.cssAll()` method like so:

~~~
var css = $('#elem').cssAll('color,backgroundColor,opacity,height,lineHeight');
~~~

By using the colon (`:`) we can also map a property to a name to make it a little easier to assign values to our second element:

~~~
var css = $('#elem').cssAll('color,backgroundColor,opacity,
height,lineHeight:height');
~~~

A full example might look something like this:

~~~
<div id="cssAll" style="color:red;backgroundColor:blue;height:20px;">First Div</div>
<div id="cssAll2">Second Div</div>


<script type="text/javascript">
    var cssAll = $('#cssAll').cssAll('color,backgroundColor,opacity,height,lineHeight:height');
    $('#cssAll2').css(cssAll);
</script>
~~~

Here is the code for the method below:

~~~
(function($)
{
    $.fn.cssAll = function(css)
    {
        var obj = {};

        if(this.length)
        {
            var css = css.split(',');
            var params = [];

            for(var i=0,ii=css.length; i<ii; i++)
            {
                params = css[i].split(':');

                obj[$.trim(params[0])] = $(this).css($.trim(params[1] || params[0]));
            }
        }

        return obj;
    };
})(jQuery);
~~~