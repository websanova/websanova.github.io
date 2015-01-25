---
layout: default
title: 10 Simple Guidelines for Writing jQuery Plugins
keywords: guidelines, jquery, plugins, writing
description: Article discussing 10 simple guidelines for writing elegant jQuery plugins.
date: Feb 11 2012
---

I've written many JQuery plugins in my time and through this experience I have learned a lot.  However I've learned the most about writing plugins trying to implement other developers plugins.  I have spent countless hours trying to hack plugins out there that to me were 90 to 95 percent complete.  There are some very simple but crucially important things to keep in mind when developing a plugin to ensure that you don't drive others crazy trying to use it.

I think in most cases going this extra mile will greatly increase the quality of your plugin and make it that much more likely that it will be recommended and flourish within the community of developers.  As such here is a set of general guidelines I follow before I wrap up my plugin for public use.

### Give Your Plugin a Unique Name

When you create your plugin there will be a way to call it, for instance if you're using jQuery it will probably be something like:

~~~
$("#container").pluginName();
~~~

Try to keep your pluign name unique and not generic like:

~~~
$("#container").tooltip();
~~~

This will avoid conflicts with any existing code or plugins that may already exists.  I would also suggest to follow a unique naming pattern for all your plugins so that they become immediately recognizable and start building up your brand.  This requires absolutely no extra effort while developing your plugin, but only some foresight ahead of time.  So for instance for the websanova brand I prepend all my plugins with a "w", like so:

~~~
$("#container").wTooltip();
~~~

This makes the plugin name unique and recognizable if someone should happen to come across one of my other plugins.

### Don't Set Any Generic Id or Class Names

This goes along with point one and follows that you should really not use any generic id or class name anywhere in your code.  I have used countless plugins that ended up conflicting with my existing CSS because of a generic word like "button".  The safest bet is to prepend all classes and ids with your plugin name.  I also even go as far as prepending an underscore just to make sure there are no conflicts.

For example:

~~~
_wTooltip_button
_wColorPicker_active
_wPaint_menuHolder
~~~

I would say the odds will be pretty good that the above class or id names will not conflict with any existing classes or ids.  A small detail like this can make or break your plugin and requires almost no extra effort on your part other than a little bit more typing.

### Keep CSS and Javascript Code Separate

You never know what someone will want to modify within your code.  Unless it is absolutely necessary to have a style associated with an element, keep all your styles in a CSS file.  This is a little check I always do before I wrap up my plugin for public use, just a quick run down to see if I hard coded any CSS styles in haste.  Making it very easy for developers to quickly modify your plugin via CSS will ensure your plugin is reused and recommended to others.

### Structure Your Files

It would seem that this one should go without saying, but I've seen some pretty bizzare setups for some of the plugins out there that really made me scratch my head.  I think the best way to let the developer know what's going on with your plugin is to follow a simple file structure like the one below:

~~~
./images
./inc
./themes
demo.html
wTooltip.css
wTooltip.js
~~~

Looking at this I know which files are the actually plugin, which ones I need to include and where additional resources like images and themes can be found.

### Don't Hard Code Any Paths

I've seen this one far too often, developers hard coding paths within their plugin, occassionally allowing an option to specify your own path.  I can't think of a situation where you would have to hard code a path to an image and either way the paths should always be relative.  I prefer to keep any images relative from my css, which can be done by setting images as background's rather that using the image tag.  Following the structure above if I have images I can now just set paths in my css as:

~~~
./images/icon.png
~~~

Now we don't have to worry about where the files are being kept, making our plugin work much better out of the box.

### Organize Your CSS File

Try to keep a general structure to your CSS file organizing things like layout, colors and styles.  This one is quite simple and is something I try to generally do while I'm putting my CSS code together to begin with.  I always separate my plugin CSS into three parts.

- __general layout__ - things that probably don't need to be modified
- __layout styles__ - like borders, margins or padding that a developer might want to change
- __colors__ - I like to put these into groups, like a set of borders or backgrounds that should probably all have the same color.

This makes it real easy to modify the plugins styles as I can easily navigate through the CSS to see what I'm affecting.  It also allows me to make changes much more confidently knowing that the styles are grouped and that my changes should not affect anything else.

### Don't Require a Structure

To me a plugin should just work, I specify a div, call the plugin and things just happen.  I've seen so many plugins that require the developer to setup some HTML code in a specific way for it to work.  These are things that should be coded directly into the plugin and be setup on the fly.  Also going with the next point, these provide great opportunities to provide options for your developers allowing them to set certain parts of your plugins on and off.

### Provide Some Options

There are so many plugins out there that to me seem 95% complete because they are missing a simple option to modify the plugins properties.  Something simple like being able to specify the length of a fadeout can be added very easily with almost no effort.  For instance, with my color picker plugin I provided some simple options to be able to use the plugin in flat mode or target mode.  Target mode meant that rather than inserting the color picker into a target div the color picker would appear when the target div was activated with a click or a hover (also options).  Another good example would be a slideshow plugin that allows you to toggle certain interface options like captions.  This follows from the point above and requires very little extra setup, taking the plugin from good to great.  

### Provide Minified Versions of Your Plugin Files

This is a tiny extra little step to save some time for developers and just polishes off your plugin making it look a little more professional.  It takes all of one minute to do and makes it convenient for your developers to not have to do it themselves.  I always think, I only have to do it once vs every developer out there doing it each time.  There is a great web based YUI version of the compressor that I always use that's worked well for me so far that does both JavaScript and CSS.

You can find it here: [Online YUI Compressor](http://www.refresh-sf.com/yui)

### Version and Document

Finally, before wrapping up your plugin provide some documentation.  All my plugins are free and public so I like to provide documentation in case anyone wants to modify or understand what's going on.  However you don't need to go that far, but at least provide some basic info at the top of your files, at the minimum providing a version number for your plugin.  If you ever make any updates a developer should know what version they have and which one they might want to upgrade to, don't make them guess or try to figure it out.  Here is a sample structure I generally follow:

~~~/******************************************
 * Websanova.com
 *
 * Resources for web entrepreneurs
 *
 * @author          Websanova
 * @copyright       Copyright (c) 2012 Websanova.
 * @license         This websanova tooltip jQuery plug-in is dual licensed under the MIT and GPL licenses.
 * @link            http://www.websanova.com
 * @docs            http://www.websanova.com/plugins/websanova/tooltip
 * @version         Version 1.3
 *
 ******************************************/
~~~

This lets my users know what's going on with the plugin, what the license is, version and links for more information.  It's easy to just copy and paste this and don't forget to include it in your minified versions as well.