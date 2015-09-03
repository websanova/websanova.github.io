---
layout: master
title: jQuery Humanized Message Plugin
keywords: jquery, humanized, message, msg, plugin
description: Sample code an demo for the Websanova jQuery Humanized Message plugin.
date: Apr 10 2012
permalink: /blog/jquery/jquery-humanized-message-plugin
redirect_from: /tutorials/jquery/jquery-humanized-message-plugin.html
---

This post is to introduce the newest Websanova jQuery Humanized Message Plugin.  It's a very sleek way to display a status message/feedback to the user rather than updating some little div on the page.  This message pops up nice and clearly and can't be missed.  Something similar is used on Twitter which was the initial inspiration to build it out as we have adopted it's use in many of our own apps.

This plugin was built using the [Websanova jQuery Plugin Development Boilerplate](http://wboiler.websanova.com) which is proving to be very useful as it only took about 2 hours to write this plugin using the boilerplate.

- [Documentation](https://github.com/websanova/wHumanMsg)
- [Live Demo](http://whumanmsg.websanova.com)

The plugin comes with plenty of defaults and should be ready to go out of the box, it requires one step to setup before you can start displaying messages.

~~~
var hm = $("#container").wHumanMsg();   // setup
hm.wHumanMsg("Display my message");     // display messages
~~~

The message starts in fixed mode meaning it will always stay in the same place as you are scrolling the page, but this can be changed along with it's top offset.

~~~
$("#container").wHumanMsg({
    fixed: false,
    offsetTop: 30
});
~~~

It also comes with 9 color options that can be set so that you can display different types of messages, for instance error or success.

~~~
$("#container").wHumanMsg({
    color: "red"
});
~~~

You can also set options on the fly with the message that will not affect the defaults.

~~~
var hm = $("#container").wHumanMsg();                   // setup

hm.wHumanMsg("Display a message", {color: "green"});    // display as green
hm.wHumanMsg("Display a message", {fadeOut: 500});      // fade out in 500 milliseconds

hm.wHumanMsg("Display a message");                      // this will return to defaults
~~~

There are many sites starting to adopt this style of message prompting or something similar to it, particularly with mobile development where little updates under the submit button are easily missed.  For more documentation and to see a live demo visit the links below.