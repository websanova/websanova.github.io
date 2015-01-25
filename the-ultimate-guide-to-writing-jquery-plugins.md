---
layout: default
title: The Ultimate Guide to Writing jQuery Plugins
keywords: jquery, plugin, development, guide
description: An article outlining in detail jQuery plugin development.
date: Oct 16 2010
---

I have written a few articles about jQuery Plugin Development in the past which readers seem to have been interested in, but I have since then written many more plugins and have thus picked up a few more tips and tricks.  I have therefore decided to put together this more comprehensive guide on jQuery Plugin Development.

I also want to note the importance of knowing how to develop jQuery plugins well as I find myself building many little custom plugins for my sites making it much easier to maintain not to mention reuse in other projects.  Anything from little pagination and nav loaders to larger handlers for many of my ajax requests.  When using in conjunction with a client side framework like Backbone.js you can really develop some nice modular code and keep frameworks like Backbone doing what they do best, controlling the logical flow of your website.

I have a nice [jQuery Plugin Development Boilerplate](http://github.com/websanova/boilerplate) on GitHub which I use as a starting point for all my plugins, I keep it updated with all the latest little tips and tricks I pick up along the way.  But without further adieu, in no particular order:

### Keep All Your Code in a Closure

This is more of a general guideline for JavaScript development all together, but I still do see plugins and code out there that break this rule.  You should always keep your code in a closure to safely avoid any namespace conflicts.

~~~
(function($){
    //code here
})(jQuery);
~~~

### Create a Unique Name For Your Plugin

Give your plugin a fancy name and always do a search on Google for it especially if you intend on releasing it publicly.  I typically but a "w" in front of all my plugins, which helps developers know they are using a Websanova plugin.  But it's a good idea to avoid generic terms like `tooltip` or `selectbox` as we also want to avoid any conflicts with existing plugins that may exist.

~~~
$.fn.wTooltip = function(option, settings){
    //code here
}
~~~

### Maintain jQuery Method Chaining

We want to make sure that if a user is calling our plugin they can continue to call other jQuery methods afterwards should they need to.

~~~
$.fn.wTooltip = function(option, settings)
{
    return this.each(function()
    {
        //code here
    });
}
~~~

### Provide Default Settings

Provide a clear area for all your plugin settings, internal or external in a variable that can itself be modified before instantiation.  This let's us change the settings as a whole rather then having to pass modified values each time we instantiate our plugin.  Also make sure to provide some comments for what exactly it is that option is for.  Having all of these things in one area makes it much easier to maintain and follow.  

~~~
$.fn.pluginName.defaultSettings = {
    theme     : 'red',       // color theme for plugin
    onClick   : null         // menu click callback
};
~~~

### Create a Local Copy of Settings

Of course we will also want to extend these settings with ones users can pass in on the fly by using the jQuery `$.extend` function.  We will want to create a local copy within the main element loop itself in order to avoid clobbering other elements settings.

~~~
$.fn.pluginName = function(option, settings)
{
    return this.each(function()
    {
        var _settings = $.extend({}, $.fn.pluginName.defaultSettings, settings || {});
        //code here
    });
}
~~~

### Setup Class Prototyping

A prototype extends an object by providing it methods without having to instantiate a copy in each object.  This is generally a good Object Oriented approach to take in JavaScript as it helps organize your code and keeps it more modular.  However, there are two main reasons why using prototyping in JavaScript is a good idea:

- It saves lots of memory by not having to recreate all applicable methods for every instance of the object you create.
- Referencing an existing function is faster than creating a new one, regardless of memory usage.

There are two parts to setting up a `prototype` function. First we need to create our initial `class` definition (which is really just another object). This `class` object will contain code that is unique to each instance of that object.

~~~
function PluginName(settings, $elem)
{
    this.settings = settings;
    this.$elem = $elem;
    this.$el = null;

    // make sure to return the object so we can reference it later
    return this;
}
~~~

We then `prototype` our function `class` which will contain the global methods available to each instance of that object.

~~~
PluginName.prototype = 
{
    generate: function()
    {
        //generate code
    }
}
~~~

### Using `this` Object Reference


Most developers will be aware of how to correctly use the `this` object but I thought it would be a good idea to quickly cover it here.  There will be many parts of your plugin that reference closures with there own local version of `this` that don't reflect the plugins version.  An example may be creating a `click` event on an element.  In order to maintain both references you will need to create a local copy of `this` which I like to assign as `_self`.  Sometimes you will see developers using `$this` however I prefer to use `$` to denote jQuery objects.  Also note the use of the `apply` function here, which lets us to properly pass along the `this` reference from within the `click` closure.

~~~
PluginName.prototype = 
{
    generate: function()
    {
        var _self = this;

        $('elem').click(function(){
            //using this will not be found since it has it's own this
            //use _self instead.

            _self.someFunc.apply(_self, ['red']);
        });
    },

    someFunc: function(color)
    {
        this.$elem.css('color', color);
    }
}
~~~

### Keep a Copy of the Settings and Element In Each Object

You will always want to keep a copy of the settings in each object rather than trying to pass around arguments in each method.  It is much cleaner this way and makes your settings available anywhere in your plugin object.  Likewise, it's a good to keep the elements reference handy as in many cases our plugin won't be influencing a the instantiating element directly but rather a new element that is created like in the case of a tooltip.  This lets us remember which element that plugin is working on.

~~~
$.fn.pluginName = function(option, settings)
{
    return this.each(function()
    {
        var $elem = $(this);
        var _settings = $.extend({}, $.fn.wBoiler.defaultSettings, settings || {});

        var plugin = new PluginName(_settings, $elem);

        // code here
    });
}

function PluginName(settings, $elem)
{
    this.settings = settings;
    this.$elem = $elem;

    return this;
}
~~~

### Prefix All `class` and `id` Names

This is just a quick note on naming conventions in your plugin.  Much like naming your plugin it is good practice to provide unique names for all your `class` and `id` names by prepending all of them with your plugins unique name.  This will avoid you running into any conflicts with other CSS rules a developer may have set.  Use a prefix like `_wTooltip_theme` rather than just `theme` on it's own.

### Save a Reference of Your Plugin Object

Once our plugin is instantiated we will need to keep a reference of it in case we need access or update it.  We could store it in an array somewhere but this would force developers to do more work and jQuery has provided some nice functionality for us out of the box to let us do just this with the `data` method.  This allows us to keep a copy of the plugin instance right with the element.  Don't forget to keep the data reference name unique and consistent with your plugin naming convention.

~~~
$.fn.pluginName = function(option, settings)
{
    return this.each(function()
    {
        var $elem = $(this);
        var _settings = $.extend({}, $.fn.wBoiler.defaultSettings, settings || {});

        var plugin= new PluginName(_settings, $elem);

        //some code

        $elem.data('_pluginName', plugin);
    });
}
~~~

### Provide Setter and Getter Options

Sometimes your plugins properties will need to change after instantiation and you will need to update a setting or call a method within the plugin object.  We can do this by providing some simple setter/getter logic at the top of our plugin that first checks if any plugin data is set for that element and then attempts to update a setting or run a specific method.  If we are getting values we will either return an array of values if multiple elements are referenced or return a single value for that one element.

~~~
$.fn.wBoiler = function(option, settings)
{
    if(typeof option === 'object')
    {
        settings = option;
    }
    else if(typeof option === 'string')
    {
        var values = [];

        var elements = this.each(function()
        {
            var data = $(this).data('_wBoiler');

            if(data)
            {
                if(option === 'reset') { data.reset(); }
                else if(option === 'theme') { data.setTheme(settings); }
                else if($.fn.wBoiler.defaultSettings[option] !== undefined)
                {
                    if(settings !== undefined) { data.settings[option] = settings; }
                    else { values.push(data.settings[option]); }
                }
            }
        });

        if(values.length === 1) { return values[0]; }
        if(values.length > 0) { return values; }
        else { return elements; }
    }

    return this.each(function()
    {
        //code here	
    });
}
~~~

### Keep Your Prototype Function Logic Clean

This is more of a code design related issue and not so specifically about plugin development, but I wanted to highlight a point as it may save you time down the road.  It's good to keep your logic as separated as possible, particularly for things like updating colors, or other visuals on your plugin.  Separate your code into nice smaller logical units and try to reuse it as much as possible.  As your plugin grows and you need to add more features it will give you less headache down the road when these functions are already available.

~~~
PluginName.prototype = 
{
    generate: function()
    {
        var _self = this;

        _self.setTheme(this.settings.theme);
        _self.setColor(this.settings.color);
    },

    setColor: function(color)
    {
        _self.$elem.css('color', color);
    },

    setTheme: function(theme)
    {
        _self.$elem.attr('class', '_pluginName_theme' + theme);
    },

    showElem: function(){},
    hideElem: function(){},
    appendComponents: function(){},
    //etc...
}
~~~

### Add Mobile Events

Occasionally you may need to provide some mobile events for your plugin.  We can do this with a little bit of code that dynamically assigns any element some mobile events for `touchstart`, `touchmove`, `touchend` and `touchcancel` in place of `mousedown`, `mousemove` and `mouseup` respectively.  Notice the `preventDefault` option that lets us set whether any other events on that element should be executed.

~~~
PluginName.prototype = 
{
    generate: function()
    {
        // code here	
    },

    bindMobile: function($el, preventDefault)
    {
        $el.bind('touchstart touchmove touchend touchcancel', function ()
        {
            var touches = event.changedTouches, first = touches[0], type = ""; 

            switch (event.type)
            {
                case "touchstart": type = "mousedown"; break; 
                case "touchmove": type = "mousemove"; break; 
                case "touchend": type = "mouseup"; break; 
                default: return;
            }

            var simulatedEvent = document.createEvent("MouseEvent"); 

            simulatedEvent.initMouseEvent(
                type, true, true, window, 1, 
                first.screenX, first.screenY, first.clientX, first.clientY, 
                false, false, false, false, 0/*left*/, null
            );

            first.target.dispatchEvent(simulatedEvent);
            if(preventDefault) event.preventDefault(); 
        });
    }
}
~~~

### Provide a `destroy` Method

It's not very often, but sometimes it's nice to have a destroy method for our plugin that completely removes it.  It's a nice way to polish off our plugin and can be accomplished quite easily so it's a good idea to tuck it away into your plugin somewhere.  Usually it just requires us to remove any elements and data using jQuery's `remove` and `removeData` methods.

~~~
PluginName.prototype = 
{
    generate: function()
    {
        this.paginate = $('<div class="_wPaginate_holder"><div>');	
    },

    destroy: function()
    {
        this.paginate.remove();
        this.$elem.removeData('_wPaginate');
    }
}
~~~

### Provide Themes

This is another point on code design but again one that may save you some headache down the road.  Most of your plugins will have some kind of appearance, look and feel.  I find it's a good idea to always provide a theme even if it's only one as a default.  The top most element of your plugin should then contain a class name of `_pluginName_theme_default` where default can be any theme name like `red`, `classic` or `neopolitan` if you prefer.  I then like to provide a separate section in my CSS file just for themes which makes it super easy for a developer to try different themes and to roll their own.

~~~
// layout and other styles here

//themes - default
._pluginName_default ._pluginName_mainHolder{color:#333; background-color:#CACACA;}
._pluginName_default ._pluginName_button{color:#FFF; background-color:#6699FF;}

._pluginName_red ._pluginName_mainHolder{color:#FF0000; background-color:#CACACA;}
._pluginName_redt ._pluginName_button{color:#FFF; background-color:#FF0000;}

//etc...
~~~

### Provide Version Information

This is usually the last thing I do after creating or updating a plugin and that is to provide or update some basic info on the plugin.  I always increment the version which helps developers know which version they have so they can know if they need to upgrade or not.  It's also a good idea to provide a link to your plugin page and other resources.

~~~
/******************************************
 * Websanova.com
 *
 * Resources for web entrepreneurs
 *
 * @author          Websanova
 * @copyright       Copyright (c) 2012 Websanova.
 * @license         This websanova jQuery boilerplate is dual licensed under the MIT and GPL licenses.
 * @link            http://www.websanova.com
 * @github          http://github.com/websanova/boilerplate
 * @version         1.2.3
 *
 ******************************************/
~~~

### Structure Your Plugin Files

Another fundamental code design issue, but amazingly one that I see missed far too often.  Organize your files in a simple clean way rather than dropping a jumble of files into the root folder.  A basic setup with an `image` and `inc` (short for includes) folder will suffice.  I also always like to provide an `index.html` file with a bunch of examples, usually this is where I do my testing for the plugin anyway.

~~~
/images
/inc
index.html
README
pluginName.css
pluginName.js
~~~