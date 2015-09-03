---
layout: master
title: 10 Coding Tips to Write Superior jQuery Plugins
keywords: jquery, plugins
description: A guide with some useful tips for writing elegant jQuery plugins.
date: Feb 25 2012
permalink: /blog/jquery/10-coding-tips-to-write-superior-jquery-plugins
redirect_from: /tutorials/jquery/10-coding-tips-to-write-superior-jquery-plugins.html
---

Through the process of writing many jQuery plugins I have come to a point where I follow a pretty standard structure and design pattern when developing my plugins.  It for one greatly speeds up my development time as it's just one part of the equation I don't need to think about and can just copy and paste.  I already know how things will be structured and will work for the most part so I can focus on just building out the main code.

Following a consistent design pattern and structure also helps when fixing bugs or building on top of a plugin.  Having a structure that has proved to stand well in my other plugins means I won't have to rehash my code to account for new details.  I know it can hold well with small plugins as well as larger ones.

I wanted to share this set of coding tips that I follow and generally from what I can tell are pretty consistent among the more popular plugins out there.

## Keep All Your Code in a Closure

This one is usually followed most of the time, but I have seen this broken with functions existing outside of the closure.  Your plugin code will only need to exist for the plugin itself, so it's a good idea to keep all your code within the closure, any functions should probably exist within a Prototype function anyway which we'll cover later.

~~~
(function($)
{
    //code here
})(jQuery);
~~~

## Provide and Extend Default Options

Your plugin will most likey have some options that developers can set so it's a good idea to have a set of default options to fall back on.  You can then extend these options using the jQuery extend function.

~~~
var defaultSettings = {
    mode            : 'Pencil',
    lineWidthMin    : '0',
    lineWidthMax    : '10',
    lineWidth       : '2'
};

settings = $.extend({}, defaultSettings, settings || {});
~~~

## Always Return the Element

One of the great features of JavaScript/jQuery is that you can just chain one function after another, therefore we should always return the element to not break this flow.  This following is a pretty standard setup to use for any jQuery plugin.

~~~
$.fn.wPaint = function(settings)
{
    return this.each(function()
    {
        var elem = $(this);

        //run some code here
    });
}
~~~

This also allows you to send multiple elements in at once which also adheres to the method chaining model.

## Keep Single Use Code Outside Main Loop

This one is pretty important and often missed.  Basically if you have code like a set of defaults it only needs to be instantiated once rather than on each call of the plugin function.  This makes your plugin more efficient and makes it use less memory.  We will see this in action more with the use of prototypes later on.

~~~
var defaultSettings = {
    mode            : 'Pencil',
    lineWidthMin    : '0',
    lineWidthMax    : '10',
    lineWidth       : '2'
};

$.fn.wPaint = function(settings)
{
    settings = $.extend({}, defaultSettings, settings || {});

    return this.each(function()
    {
        var elem = $(this);

        //run some code here
    });
}
~~~

Notice above how the "defaultSettings" are completely outside the plugin function, because they are in a closure we don't have to worry about this variable getting overwritten by another plugin.

## Setup Class Prototyping - The Why

The meat of your code, the functions and methods should exist within a prototype function.  There are two main reasons for this as follows:

1. It saves lots of memory by not having to recreate all applicable methods for every instance of the object you create.
2. Referencing an existing function is faster than creating a new one, regardless of memory usage.

A prototype basically extends an object by providing it methods without having to instantiate a copy in each object.  It also serves as a good way to organize your code more efficiently and once you get into a habit of using this structure you will find it significantly reduces your development time in future projects.

## Setup Class Prototyping - The How

There are two parts to setting up a prototype function.  First we need to create our initial "class" definition, this is meant in the most general sense as it's really just an object.  This initial "class" object will contain the code that is unique to each instance of that object For instance in my [Paint jQuery Plugin](http://wpaint.websanova.com) I setup a canvas object similar to something below:

~~~
function Canvas(settings)
{
    this.settings = settings;
    this.draw = false;
    this.canvas = null;		
    this.ctx = null;

    return this;
}
~~~

We can then prototype it making the methods globally available for each Canvas object.

~~~
Canvas.prototype = 
{
    generate: function()
    {
        //generate code
    }
}
~~~

The key is to make sure the functions in the prototype are generic and all data unique to each instance of the canvas should be stored using the "this" object.

## Using the "this" Object

I thought it would be important to briefly go over using the "this" object within the prototype.  Basically this allows you to call any methods or variables for each unique instance of our object, but we have to be weary of using it within other closures like event functions.

~~~
Canvas.prototype = 
{
    generate: function()
    {
        //some code

        var $this = this;

        var buton = //...some code

        button.click(function(){
            //using this will not be found since it has it's own this

            //use $this instead.

            $this.someFunc($this);
        });
    },

    someFunc: function($this)
    {
        //won't know what 'this' is.
        //use $this instead passed from the click event
    }
}
~~~

By using $this, we are able to pass in the correct reference to use within other closures.  We may also need to pass the $this reference into other methods as well.  Note that $this is completely arbitrary and any variable name could be used.

## Keep Your Settings in Each Object

I always keep the settings in each object by passing in it's own copy and manipulating it directly there.  This will keep you from passing arguments around from function to function.  By keeping it in the object you can easily call it from anywhere and it's always available.

~~~
function Canvas(settings)
{
    this.settings = settings;

    return this;
}
~~~

## Separate Your Prototype Function Logic

This is a general principle that will probably only come with experience but I will attempt to give a few tips here.

Whenever you are writing a function and wondering what should be included in it or not, always ask yourself "if someone were to rewrite this function would the existing code still work?".  Meaning if someone wanted to write there own version of a menu, or add an effect or theme how difficult would this be for them.  Of course there are varying degrees on how dynamic you really need to go.  Here is my function list from my [Color Picker jQuery Plugin](http://wcolorpicker.websanova.com) to help give you an idea:

~~~
generate()
appendColors()
colorSelect()
colorHoverOn()
colorHoverOff()
appendToElement()
showPalette()
hidePalette()
~~~

## Provide a Setter/Getter Option

This one will not always be required but I find myself including it in all of my plugins now as it's a very small amount of code and provides the option should someone need it.

Basically we just want to be able to provide the developer with the ability to set or get existing values from the element using the plugin function like so:

~~~
var lineWidth = $('#container').wPaint('lineWidth');
$('#container').wPaint('lineWidth', '5');
~~~

First we need to attach the object to the element so that we can reference it at a later time.  We would do this at the end of our each loop from above, just before returning the element.

~~~
return this.each(function()
{
    var elem = $(this);

    var canvas = new Canvas(settings);

    //run some code here

    elem.data('_wPaint_canvas', canvas);
});
~~~

Now we need some code to be able to determine what we're trying to do

~~~
$.fn.wPaint = function(option, settings)
{
    if(typeof option === 'object')
    {
        settings = option;
    }
    else if(typeof option === 'string')
    {
        if(
            this.data('_wPaint_canvas') &amp;&amp;
            defaultSettings[option] !== undefined
        ){
            var canvas = this.data('_wPaint_canvas');

            if(settings)
            {
                canvas.settings[option] = settings;
                return true;
            }
            else
            {
                return canvas.settings[option];
            }
        }
        else
            return false;
    }

    return this.each(function()
    {
        //run some code here
    });
}
~~~

## Update: Make Your Default Settings Global

Some plugins you may want to make your default settings global allowing a developer to setup their own defaults.  This can be done quite easily like so:

~~~
(function($)
{
    $.fn.wTooltip = function(option, settings)
    {
        settings = $.extend({}, $.fn.wTooltip.defaultSettings, settings || {});

        // plugin code
    };

    $.fn.wTooltip.defaultSettings = {
        position    : 'mouse',
        color       :'black'
    };

    //Prototype/classes here
})(jQuery);
~~~

Now a developer can set whatever default they like by doing the following:

~~~
$.fn.wTooltip.defaultSettings.color = 'white';

$('#container').wTooltip(); // call pluign normally
~~~

This gives your plugin a nice little extra bit of flexibility should you want to provide it.  One could also imagine a hybrid scheme with both global and private defaults where you would extend the settings twice, however that would seem like a very rare and specific scenario.

## Update: Make a Local Copy of Settings

If you find your plugin is making a lot changes to the settings it may be important to keep a local copy of each set of settings for each plugin instance.  If for instance we are setting a title in a plugin on multiple elements it will loop through setting the last title as the settings.title value.  If we want to then access a previous elements title attribute we would get the last set value instead of the value we are expecting.

~~~
(function($)
{
    $.fn.wTooltip = function(option, settings)
    {
        settings = $.extend({}, $.fn.wTooltip.defaultSettings, settings || {});

        return this.each(function()
        {
            var $settings = jQuery.extend(true, {}, settings);

            //code
        }
    };

    //Prototype/classes/defaults here
})(jQuery);
~~~

This should pretty much cover the core of your jQuery plugin development and acts as a nice template to follow.  Having a base set of code to use greatly decreases development time and gives you a default way to architect your plugins confidently knowing that you can tackle any situation you may encounter.