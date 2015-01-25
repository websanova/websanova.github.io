---
layout: default
title: Extending JavaScript - The Right Way
keywords: extending, javascript, right, way
description: An article highlighting how to extend JavaScript correctly to avoid issues with namespacing and the global object scope.
date: Apr 3 2012
---

JavaScript comes with a lot of great functionality built in, but what if there is a function you need which is missing.  How can we build them in seamlessly in an elegant way that extends the functionality of our beloved JavaScript.  The following will outline a couple methods to extend the existing functionality of JavaScript, both effective but one a little more functionally complete.

Say we want to create a function .capitalize() to extend the String type of JavaScript so that it behaves in a way similar to `String.toLowerCase()` or `String.toUpperCase()`.  We can do this by prototyping the String object adding in our new function.

This is usually done in the simplest way with the code below:

~~~
if(!String.prototype.capitalize)
{
    String.prototype.capitalize = function()
    {
        return this.slice(0,1).toUpperCase() + this.slice(1).toLowerCase();
    }
}
~~~

This works fine and dandy but lets say we were to do the following:

~~~
var strings = "yay";
for(i in strings) console.log(i + ":" + strings[i]);
~~~

We would get the output:

~~~
0: y
1: a
2: y
capitalize: function () { return this.slice(0, 1).toUpperCase() + this.slice(1).toLowerCase(); }
~~~

Our capitalize function shows up in our for loop, which is correct since it is now a property of all strings.  The reason it is doing this is because the enumerable property for our new function is by default set to true.

However, this was wreaking havoc on a plugin I had written that happened to be iterating through each character in a string.  By simply changing the enumerable property to false we can avoid this problem which can be done by using the defineProperty method like so: 

~~~
if(!String.prototype.capitalize)
{
    Object.defineProperty(String.prototype, 'capitalize',
    {
       value: function()
       {
           return this.slice(0,1).toUpperCase() + this.slice(1).toLowerCase();
       },
       enumerable: false
    });
}
~~~

Now when we run our loop we get the outcome we were more likely expecting.

~~~
var strings = "yay";
for(i in strings) console.log(i + ":" + strings[i]);
~~~

~~~
0: y
1: a
2: y
~~~

The key thing to note is that just because we can't see it in the for loop, doesn't mean it's not there, just like any other property,  we can call it, change it or delete it.

~~~
var strings = "yay";
console.log(strings.capitalize)
~~~

~~~
function () { return this.slice(0, 1).toUpperCase() + this.slice(1).toLowerCase(); }
~~~

The reason for this may not seem as obvious when we're looking at strings, but it gives us a lot of flexibility should we need it.  It can come in really handy when defining our own objects and setting some default values that we would want to expose.

Below are just a few more examples you may wish to use in some of your own projects:

### String.pxToInt()

Convert css px value like "200px" to an Integer.

~~~
if(!String.prototype.pxToInt)
{
    Object.defineProperty(String.prototype, 'pxToInt',
    {
        value: function()
        {
            return parseInt(this.split('px')[0]);
        },
        enumerable: false
    });
}
~~~

### String.isHex()

Checks if string is valid Hex value such as `#CCC` or `#CACACA`.

~~~
if(!String.prototype.isHex)
{
    Object.defineProperty(String.prototype, 'isHex',
    {
        value: function()
        {
            return this.substring(0,1) == '#' &&
                   (this.length == 4 || this.length == 7) &&
                   /^[0-9a-fA-F]+$/.test(this.slice(1));
        },
        enumerable: false
    });
}
~~~

### String.reverse()

Reverse a string.

~~~
if(!String.prototype.reverse)
{
    Object.defineProperty(String.prototype, 'reverse',
    {
        value: function()
        {
            return this.split( '' ).reverse().join( '' );
        },
        enumerable: false
    });
}
~~~

### String.wordCount()

Count the number of words in a given string, words being separated by spaces.

~~~
if(!String.prototype.wordCount)
{
    Object.defineProperty(String.prototype, 'wordCount',
    {
        value: function()
        {
            return this.split(' ').length;
        },
        enumerable: false
    });
}
~~~

### String.htmlEntities()

Converts HTML characters like `<` and `>` to HTML encoded special characters.

~~~
if(!String.prototype.htmlEntities)
{
    Object.defineProperty(String.prototype, 'htmlEntities',
    {
        value: function()
        {
            return String(this).replace(/&/g, '&amp;').replace(/\</g, '&lt;').replace(/\>/g, '&gt;').replace(/"/g, '&quot;');
        },
        enumerable: false
    });
}
~~~

### String.stripTags()

Strips out all HTML tags from the string.

~~~
if(!String.prototype.stripTags)
{
    Object.defineProperty(String.prototype, 'stripTags',
    {
        value: function()
        {
            return this.replace(/\<\\/?[^\>]+\>/gi, '');
        },
        enumerable: false
    });
}
~~~

### String.trim()

Removes all leading and trailing white space from string.

~~~
if(!String.prototype.trim)
{
    Object.defineProperty(String.prototype, 'trim',
    {
        value: function()
        {
            return this.replace(/^\s*/, "").replace(/\s*$/, "");
        },
        enumerable: false
    });
}
~~~

### String.stripNonAlpha()

Removes all non-alphanumeric characters from string.

~~~
if(!String.prototype.stripNonAlpha)
{
    Object.defineProperty(String.prototype, 'stripNonAlpha',
    {
        value: function()
        {
            return this.replace(/[^A-Za-z ]+/g, "");
        },
        enumerable: false
    });
}
~~~

### Object.sizeof()

The the size of an object, for example: {one: "and", two: "and"} would equal 2

~~~
if(!Object.prototype.sizeof)
{
    Object.defineProperty(Object.prototype, 'sizeof',
    {
        value: function()
        {
            var counter = 0;
            for(index in this) counter++;
            
            return counter;
        },
        enumerable: false
    });
}
~~~