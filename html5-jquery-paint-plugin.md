---
layout: default
title: HTML5 jQuery Paint Plugin
keywords: html5, jquery, wpaint, paint, plugin
description: Sample code and demo of the Websanova wPaint jQuery plugin.
date: Apr 17 2012
---

Websanova Paint is a HTML5 canvas based jQuery plugin.  It allows you to free paint on a canvas area with various shapes and colors including an eraser.  It also features the fantastic [Websanova Color Picker](http://wcolorpicker.websanova.com) allowing you to set both border and fill colors.  The canvas area can be set to any size you like and perhaps it's greatest feature is the ability to save your drawing as an image and then load it back in later.  In fact you can load any image in as your drawing as long as it is a base64 encoded PNG image.

Here are it's features outlined below:

- Menu - Can be moved anywhere on the screen.
- Shapes - Ellipse, Rectangle, Line, Pencil and Eraser.
- Colors - Select colors for both border and fill, featuring the [Websanova Color Picker](http://wcolorpicker.websanova.com).
- Width - Set the line or border width of the pencil and shapes.
- Save/Load - You can save your creations and load in any image.

The plugin was designed using the [Websanova jQuery Plugin Development Boilerplate](http://wboiler.websanova.com).

- [Documentation](https://github.com/websanova/wPaint)
- [Live Demo](http://wpaint.websanova.com)

Setup is quite simple, the canvas will fill the area of it's container.

~~~
$("#container").wPaint();
~~~

You can set the min and max pencil width along with some defaults for the menu.

~~~
$("#container").wPaint({
    lineWidthMin: 5,
    lineWidthMax: 10,
    mode: 'Pencil',
    lineWidth: 5,
    fillStyle: '#6699FF',
    strokeStyle: '#FF0000'
});
~~~

There are also handlers for events when something is being drawn.

~~~
$("#container").wPaint({
    drawDown: function(){},
    drawMove: function(){},
    drawUp: function(){}
});
~~~

We can also set and get our image data.

~~~
var wp = $("#container").wPaint({
    image: "data:image/png;base64,base64 encoded string of the image here"
});

var imageData = wp.wPaint("image");
$("#someImage").attr("src", imageData);
~~~