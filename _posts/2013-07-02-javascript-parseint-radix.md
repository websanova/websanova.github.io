---
layout: master
title: JavaScript ParseInt Radix
keywords: javascript, jquery, parseint, radix, websanova
description: Today I came across an error while I was linting my Intelligent RGB / HEX Converter Plugin code about requiring a radix value for parseInt(). After a quick look up on Google I could see that excluding this parameter could lead to some rather nasty bugs in your code.
date: July 02 2013
permalink: blog/javascript/javascript-parseint-radix
---

Today I came across an error while I was linting my [Intelligent RGB / HEX Converter Plugin](http://rgbhex.websanova.com) code about requiring a `radix` value for `parseInt()`. After a quick look up on Google I could see that excluding this parameter could lead to some rather nasty bugs in your code. The `radix` parameter is meant to specify the number system to use when parsing the value passed into `parseInt()`. So if we pass in a value of 16 then the number system used would be hexadecimal and a value of 8 would be octal.

The reason why this value is important is because in older web browsers like ie7 and ie8 any value with a leading zero automatically gets parsed as an octal value. So if you happened to pass in a string of `012` this would actually get parsed as the octal value 10.

For example if we put this in to ie7 or ie8:

~~~
console.log(parseInt('012'));      // 10
console.log(parseInt('012', 8));   // 10
console.log(parseInt(012, 8));     // 8
console.log(parseInt('012', 10));  // 12
~~~

If we’re writing an application that might by chance have leading zeros in it, we would really have a nasty bug leaving out that `radix` value. Most browsers will now treat all values as decimal using 10 as the default number system and ignoring the leading zeros. The one exception being a leading value of `0x` for hexadecimal values. So for instance the two following values will both get parsed using base 16.

~~~
console.log(parseInt('0x12'));     // 18
console.log(parseInt('0x12', 16)); // 18
~~~

However ie7 and ie8 are still in use and avoiding this bug is quite simple by just putting a 10 into the `parseInt()` parameter. I never use to be a big fan of linting but I find I use it all the time now to catch simple bugs like this. A great tool you can use for linting is called [Grunt.js](http://gruntjs.com/) which you can setup to automate tasks like linting, testing and minifying your JavaScript and CSS code. I have previously written a guide on [Setting up Grunt.js](/how-to-setup-grunt-js) that you can read on setting it up which can be a lifesaver for avoiding little bugs like the missing `radix` value.