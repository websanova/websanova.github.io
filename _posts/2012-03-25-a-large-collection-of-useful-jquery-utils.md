---
layout: master
title: A Large Collection of Useful jQuery Utils
keywords: large, collection, jquery, utils, utilities
description: A collection of useful jQuery utitilites to easily add to your projects on demand when needed.
date: Mar 25 2012
permalink: /blog/jquery/a-large-collection-of-useful-jquery-utils
redirect_from: /tutorials/jquery/a-large-collection-of-useful-jquery-utils.html
---

jQuery is a fantastic library but it needs to stay lean and small by providing only the most commonly need features.  However there are many other little utilities that I have used frequently enough and found myself always rewriting them, so I started collecting them into a little library.  Many of these are just little shortcuts to code I find myself constantly typing out, but don't think of it as a whole library you need to load into your code.  Just take the pieces you need and leave the pieces you don't need.

Note that I have wrapped many of these functions into jQuery although that is not required and other than the $.postJSON and $.stop() methods these functions can be used in any JavaScript library or just standalone all together.

### jQuery.postJSON()

Not sure why jQuery hasn't tossed this little shortcut in since they already have the $.getJSON method.  They do have a $.post method that is pretty much the same minus having to specify the dataType, but I find this is a nice little shortcut I frequently use.

~~~
$.postJSON(
    "/put/path/here",
    {val1: "Cheetos", val2: "Nachos"},
    function(response){ //on success do something }
);
~~~

### jQuery.stop()

This is another little shortcut to prevent event propagation.  It accepts options for preventDefault and stopPropgation.

~~~
$.stop(event, preventDefault, stopPropagation);

$("#container").click(function(e)
{
    $.stop(e, true, true);    
});
~~~

### jQuery.shuffleArray()

I haven't used this often, but it's great to have handy, randomly shuffle an array.

~~~
$.shuffleArray([1,2,3,4,5,6,7]); //potential output: [1,3,5,7,2,4,6]
~~~

### jQuery.reload()

This is just a shorthand function to reload the page rather than using "window.location.reload(true)"

~~~
$.reload();
~~~

### jQuery.uri()

This parses the URI component of a URL and lets you access it by it's index starting from 1.

~~~
http://www.domain.com/this/domain/rocks
    
$.uri(1); //will output this
$.uri(3); //will output rocks
~~~

### jQuery.URLParams()

I have probably rewritten this little method about 100 times, it's a simple little split string one liner that you never think to wrap in a function for reuse.  Note that it also handles hash tags.

~~~
http://www.domain.com/this/domain/rocks?param=fantastic&test=awesome#websanova

$.URLParams(); // {param: 'fantastic', test: 'awesome'}
$.URLParams('test'); // awesome
~~~

### jQuery.URLHash()

This is similar to the URLParams function except it returns the hash in a URL if any.

~~~
http://www.domain.com/this/domain/rocks?param=fantastic&test=awesome#websanova

$.URLHash(); // websanova
~~~

### jQuery.hexToRGB()

This is a nice little conversion function to have handy, it accepts a HEX or RGB string value and converts it.  Invalid values are returned false.

~~~
$.hexToRGB("#FF3388"); // rgb(255,51,136)
$.hexToRGB("#F38");    // rgb(255,51,136)
$.hexToRGB("#ZZ3388"); // false
$.hexToRGB("F38A");    // false

$.hexToRGB("rgb(22,67,234)"); // #1643EA
$.hexToRGB("rgb(22,67,274)"); // false
$.hexToRGB("rgb(22,67)");     // false
~~~

### jQuery.base64Encode()

This little function base64 encodes a string, note that it requires the string to be in utf8 format so you will need the utf8encode function as well.

~~~
$.base64Encode("encode this string"); // ZW5jb2RlIHRoaXMgc3RyaW5n
~~~

### jQuery.base64Decode()

This decodes a base64 encoded string.  Note that it also applies the utf8 decoding that was applied during the base64 encode.

~~~
$.base64Decode("ZW5jb2RlIHRoaXMgc3RyaW5n"); // encode this string
~~~

### jQuery.utf8Encode()

UTF8 encode a string, this is primarily used with the base64Encode function from above.

~~~
$.utf8Encode("utf8 encode this");
~~~

### jQuery.utf8Decode()

Decodes a UTF8 encoded string.

~~~
$.utf8Decode("utf8 encode this");
~~~

### .removeClassRegEx()

This is a great little function that can be a life saver.  I use this a lot to remove all classes starting with a particular string.

~~~
<div class="test testing leavemealone hellotest Tester"></div>

$("#container").removeClassRegEx(/test/i);  //class="leavemealone"
$("#container").removeClassRegEx(/test/);   //class="leavemealone Tester"
$("#container").removeClassRegEx(/^test/i); //class="leavemealone hellotest"
$("#container").removeClassRegEx(/test$/);  //class="testing leavemealone Tester"
~~~

### .hasClassRegEx()

Similar to the removeClassRegEx this checks to see if any class matches the regular expression.

~~~
<div class="test testing leavemealone hellotest Tester"></div>

$("#container").removeClassRegEx(/test/i);    // true
$("#container").removeClassRegEx(/test/);     // true
$("#container").removeClassRegEx(/^test/i);   // true
$("#container").removeClassRegEx(/test$/);    // true
$("#container").removeClassRegEx(/^testy$/);  // false
~~~

### .maxChars()

This is very useful to have for any input fields particularly a textarea which does not have a "maxlength" attribute.  It also allows you to specify a target that will display the characters left.

~~~
$("input").maxChars(50);
$("input").maxChars(50, $("#maxChars_counter"));
~~~

### Object.sizeof()

Yes this is just a JavaScript extension, but very useful to get the length of your objects.  Although in most cases you will have the length property set, but for objects you will not.

~~~
{cow: "moo", duck: "quack"}.sizeof(); // 2
~~~

### String.capitalize()

This is another JavaScript extension but great to have around, a simple capitalize extension to the String object.

~~~
"test".capitalize(); // Test
~~~

### String.pxToInt()

Another JavaScript extension, I found myself doing this so often when I returned CSS attributes in jQuery needing them as an integer that I wrote this little shorthand function to quickly convert them for me.  Note that this returns an integer number value.

~~~
"210px".pxToInt(); //210
$("container").css('height').pxToInt(); // 400
~~~