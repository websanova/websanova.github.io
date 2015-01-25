---
layout: default
title: HTML5 jQuery Scratch Pad Plugin
keywords: html5, jquery, scratch, pad, scratchpad, plugin
description: Sample of the jQuery wScratchpad 2.0 plugin demo and sample code.
date: Apr 4 2012
---

The Websanova Scratch Pad is a unique one of a kind plugin that allows you to add a scratching effect to images something similar to scratching a lottery ticket.  It is written using canvas so you would need an HTML5 compliant browser to use it but it does support a fallback to display a message to upgrade to a newer browser if the browser does not have canvas support.  What's really cool is that it keeps track of the percentage of area scratched so that you can trigger a callback once a user scratches a certain amount of the surface area.

Definitely a fun little plugin to entertain your users with.

- [Documentation & Download](https://github.com/websanova/wScratchPad) 
- [Live Demo](http://wscratchpad.websanova.com) 

Usage is quite simple and it is based off of the [Websanova jQuery Plugin Development Boilerplate](http://wboiler.websanova.com).  The most basic setup only requires the location of the image and the dimensions you want to use for the scratch window.

~~~
$("#wScratchPad").wScratchPad({
    width: 210,
    height: 100,
    image: "/path/to/image.jpg"
});
~~~

You can also modify the color and size of the brush with the following options:

~~~
$("#wScratchPad").wScratchPad({
    width: 210,
    height:100,
    image: "/path/to/image.jpg"
    color: "#DD3388",
    size: 5
});
~~~

There is also the option to layer an image on top of the other one and scratch that off instead of a background color, this lets you create some interesting looking scratch cards.

~~~
$("#wScratchPad").wScratchPad({
    width: 210,
    height:100,
    image: "/path/to/image.jpg"
    image2: "/path/to/image2.jpg"
    size: 5
});
~~~

A really nice feature is well is the ability to track the percentage scratch in order trigger some kind of message.

~~~
$("#wScratchPad").wScratchPad({
    width: 210,
    height:100,
    image: "/path/to/image.jpg"
    image2: "/path/to/image2.jpg"
    scratchMove: function(e, percent)
    {
        if(percent > 80)
        {
            //do something
        }
    }
});
~~~