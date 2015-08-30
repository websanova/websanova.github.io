---
layout: default
title: An Intelligent JavaScript RGB to HEX Converter
keywords: rgb, hex, convert, javascript, jquery, plugin, websanova
description: It’s not very often I need an rgb / hex converter in my apps or to convert a color code quickly, but when I do I find there are never any good tools around to do it quickly.
date: June 26 2013
---

It’s not very often I need an rgb / hex converter in my apps or to convert a color code quickly, but when I do I find there are never any good tools around to do it quickly. In fact typing in either “rgb to hex converter” or “hex to rgb converter” into Google yields results for sites that have really crappy converters that make you type in each `R`, `G`, `B` value separately. I came across needing to convert a rgb to a hex value again and finally decided to write my own little JavaScript extension for this. The extension auto detects the input format and spits out the appropriate output format. So if you put in a hex value it will spit out a properly formatted rgb value and vice versa.

* [Check out the demo page](http://rgbhex.websanova.com/)
* [Download the latest version](https://github.com/websanova/rgbHex)

**Examples:**

~~~
rgb(255,255,255)       => #FFFFFF
rgb(255,255,255);      => #FFFFFF
rgb(255,255,255,0)     => #FFFFFF
rgba(255, 255, 255)    => #FFFFFF
rgba(255, 255, 255, 0) => #FFFFFF
255,255,255            => #FFFFFF
255,255,255,0          => #FFFFFF
#FFFFFF                => rgb(255,255,255)
#ffffff                => rgb(255,255,255)
#FfFfFf                => rgb(255,255,255)
#FFF                   => rgb(255,255,255)
FFFFFF                 => rgb(255,255,255)
FFF                    => rgb(255,255,255)
~~~