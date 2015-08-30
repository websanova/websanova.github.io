---
layout: master
title: jQuery Max Character Counter Plugin
keywords: jquery, max, character, counter, plugin
description: Sample code and demo for the Websanova jquery Max Character Counter Plugin.
date: Apr 18 2012
---

This little plugin was originally designed to limit the amount of characters that can be typed into the `textarea` element which does not support the `maxlength` attribute like an `input` element does.  With HTML5 this is no longer required as they have finally added this attribute but until the rest of the world catches up I find myself still using this quite frequently as a backup.  Actually in many cases I prefer to use it over setting the `maxlength` attribute as it's easier to change if I have a large set of inputs.

There is also another feature I've added that makes it quite useful even with the addition of the HTML5 `maxlength` attribute.  You can set a target to show the amount of characters left and it will automatically count down as you type.  This is a commonly requested feature and I found myself constantly rebuilding it, so I decided to just wrap it into a nice little jQuery plugin.


Although it is meant for input fields, we can attach it to any element that supports the `keyup` attribute.  There are five ways to use the max counter outlined below.

### Default

The first is to just set a max which will do nothing but prevent a user from typing more than the specified number of characters.

~~~
<textarea id="field"></textarea>

$("#field").maxChars(200);
~~~

### Auto-Detect Target

We can specify a max counter element which will auto-detect using the id of the element and appending `_counter` to it.

~~~
<textarea id="field"></textarea>
<span id="field_counter"><span>

$("#field").maxChars(200);
~~~

### Set Multiple Elements

We can also use the methods above to set multiple fields at the same time.  As long as there is a matching `_counter` id it will use it.

~~~
<textarea id="field1" class="field"></textarea>
<span id="field1_counter"><span>

<textarea id="field2" class="field"></textarea>
<span id="field2_counter"><span>

$(".field").maxChars(200);
~~~

### Specify Target Element

If you don't want to auto-detect we can pass in a specific target element to use as the counter as well.

~~~
<textarea id="field"></textarea>
<span id="some_counter"><span>

$("#field").maxChars(200, $("#some_counter"));
~~~

### Create a Counter Dynamically

Finally we can also specify a "true" flag for the target parameter which will automatically build a counter into the textarea for us.

~~~
<textarea id="field"></textarea>

$("#field").maxChars(200, true);
~~~

### .maxChars()

~~~
(function($)
{   
    $.fn.maxChars = function(max_chars, target)
    {
        return this.each(function()
        {
            var $target = null;
            
            if(target === undefined || target === false)
            {
                var _target = $("#" + $(this).attr('id') + "_counter");
                if(_target.length > 0) $target = _target;
            }
            else if(target === true)
            {
                $target = 
                $('<div style="position:absolute;bottom:2px;right:15px;color:#BABABA;"></div>');
                
                $(this)
                .wrap('<div style="position:relative;display:inline-block;"></div>')
                .parent().append($target);
            }
            else $target = target;
            
            var mc = new MaxChars($(this), $target, max_chars);
            
            mc.count();
            
            $(this).keyup(function(){ mc.count(); });
        });
    };  
    
    function MaxChars($this, $target, max)
    {
        this.content = $this;
        this.target = $target;
        this.max = max;
        this.current = null;
        this.left = null;

        return this;
    }
    
    MaxChars.prototype =
    {
        count: function()
        {
            this.current = this.content.val().length;
            this.left = this.max - this.current;
                
            if(this.target &amp;&amp; this.target.length > 0) 
                this.target.html(this.left &lt; 0 ? 0 : this.left);
            
            if(this.current > this.max) 
                this.content.val(this.content.val().substring(0, this.max));
        }
    }
})(jQuery);
~~~

I like to collect and build little tools like this as they come in really handy and let me just toss them in.  This probably wouldn't take you more than 10 to 15 minutes to write anyway, but now you can use that time to read more articles on my site.