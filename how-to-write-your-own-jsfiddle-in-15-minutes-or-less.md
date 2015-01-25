---
layout: default
title: How to Write Your Own JSFiddle (In 15 Minutes or Less)
keywords: jsfiddle, javascript
description: This article describes how to write your own simple version of a jsfiddle style application.
date: Apr 16 2012
---

This weekend I decided to build my own version of [JSFiddle](http://www.jsfiddle.net), a small little web service that allows you to test and share HTML, CSS and JavaScript code.  It's actually quite simple to write and only requires a few lines of code.  The rest is pretty much eye candy and convenience for the user which JSFiddle has done a great job of.  Since I like to write plugins I have turned my little experiment into a jQuery plugin called JSNova.  It's only functionality is to output results and reset the boxes but it's quite fluid and you can pop it into your own projects.

- [Live Demo](http://jsnova.websanova.com)
- [Documentation](https://github.com/websanova/wJSNova)

### The Plugin

At first I had just written this out as a demo, but decided to convert it into a plugin as it was really not that much code.  Built in is the ability to select a jQuery version, a jQuery UI version and a jQuery UI theme.  You can enter HTML, CSS and JavaScript code which can be run and the resulting output is displayed.  Other eye candy like code highlighting and resizing is not built in at this time.

### The Fiddle

Really all we do is inject a string into an iFrame and it gets run as a regular web page.  So basically we just take the code from our three boxes, put the JavaScript in a `script` tag, the CSS in a `style` tag, and the HTML in the `body` tag, load it all in and voila we're done.

~~~
var result = "string";
var iframe = document.getElementById('iframe');
		
if(iframe.contentDocument) doc = iframe.contentDocument;
else if(iframe.contentWindow) doc = iframe.contentWindow.document;
else doc = iframe.document;

doc.open();
doc.writeln(result);
doc.close();
~~~

### The String

Now that we know how to insert a string you can imagine it starts becoming quite simple as we can build that string up anyway we like.  Here is a basic version of what it looks like to get everything working properly.  The one thing to note here is the placement of the `script` tag for the JavaScript code.  This is just to make it easier to be able to execute code on existing HTML.

~~~
<html>
    <head>
        <style>
            #test{background:blue;}
        </style>
    </head>
    <body>
        <div id="test">test</div>
        
        <script type="text/javascript">
            console.log('test');
        </script>
    </body>
</html>
~~~

### Adding Libraries

Adding a custom framework, libraries, themes or anything is simple as we just build it into the string just like we would a regular web page.

~~~
<html>
<head>
    <script 
        src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js" 
        type="text/javascript"></script>
    <link 
        href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8/themes/base/jquery-ui.css" 
        type="text/css" rel="stylesheet">
    <script 
        src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.8.18/jquery-ui.min.js" 
        type="text/javascript"></script>
    
    <style>
        #test{background:blue;}
    </style>
</head>
<body>
    <div id="test">test</div>
    
    <script type="text/javascript">
        console.log('test');
    </script>
</body>
</html>
~~~

It's probably a good idea to only add one library at a time like jQuery or Prototype.js for example.  You can pretty much add all the libraries you like though and it can be done easily using the [Google libraries API](https://developers.google.com/speed/libraries/devguide#chrome-frame) as your source.

### The Rest

The rest of the code is pretty much just eye candy and some server side stuff to save the fiddles.  I will outline the other pieces of the JSFiddle code, which maybe I will add some day if I'm not lazy.

### Save/Update/Fork

This is where the server side part comes in.  But all we're doing here is saving and retrieving strings from the server.  For a fork we duplicate the record, for an update we increment a version number, can't get much easier than that.

### Resizing the Boxes

One piece which I may add (don't hold me to this) is the ability to resize the code boxes.  These are based on a percentage so that they keep there size relative to each other when resizing the browser.  This would be just a little bit of JavaScript and if I get some requests I may toss it in at a later time.

### Some Eye Candy

One last piece to add would be a syntax highlighter for the code boxes.  There are plenty out there and it wouldn't be too difficult to intergrate one of them.  Also we would probably also need to add some key press handlers for the tab button to give us nice tab indents rather than switching to another portion of the page.

That pretty much covers it, sometimes it really is quite amazing how much you can accomplish with so little code and it shows you that creativity and ingenuity is just as important as being a solid developer when it comes to ideas.
