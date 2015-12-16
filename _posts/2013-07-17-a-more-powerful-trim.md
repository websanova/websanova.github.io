---
layout: master
title: A More Powerful Trim()
keywords: javascript, jquery, trim, websanova
description: I came across the situation the other day where I needed to trim a string. However I didn’t need to trim white space characters, I needed to trim some dollar ($) sign characters.
date: July 17 2013
img: /wp-content/uploads/2013/07/a-more-powerful-trim.png
permalink: /blog/javascript/a-more-powerful-trim.html
redirect_from:
  - /a-more-powerful-trim.html
---

I came across the situation the other day where I needed to trim a string. However I didn’t need to trim white space characters, I needed to trim some dollar (`$`) sign characters. So I looked up the `$.trim()` function on the jQuery docs page and was a bit surprised that they didn’t offer any arguments to pass here for the character to trim. So like I always do in these situations I decided to roll my own little trim function. The function allows any character to be passed in and by default uses white space characters if no character is set.

You can get the function on our [JavaScript Extensions Project](https://github.com/websanova/wExtensions) page.

## Examples

~~~
'   hello world   '.trimChar();    // 'hello world'
'   hello world   '.trimChar(' '); // 'hello world'
'ssshello worldsss'.trimChar('s'); // 'hello world'
'$$$hello world$$$'.trimChar('$'); // 'hello world'
'+++hello world+++'.trimChar('+'); // 'hello world'
~~~