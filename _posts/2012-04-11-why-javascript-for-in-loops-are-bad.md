---
layout: master
title: Why JavaScript For In Loops Are Bad
keywords: javascript, for, forin, loops, bad
description: An article discussing the pitfalls of using for-in loops in JavaScript and how to avoid them.
date: Apr 11 2012
permalink: /blog/javascript/why-javascript-for-in-loops-are-bad.html
redirect_from: /tutorials/javascript/why-javascript-for-in-loops-are-bad.html
---

I recently wrote an article called [Extending JavaScript - The Right Way](/extending-javascript-the-right-way) which brought up some interesting points about JavaScript and how it's prototyping model works.  However there was a piece I missed about the use of `for` loops in JavaScript and how they are basically not a good idea to use and should be avoided as much as possible.  I wanted to write a follow up piece to that article explaining the difference between using a regular `for` loop and a `for in` loop as I have received a lot of great feedback from the community.

### Extending the Prototype

My original beef with the prototype was in the fact that when you extended an object type like `String` it automatically had those properties and they could be seen if you iterated through a string using a `for in` loop.

~~~
String.prototype.test = function(){ return "test" }

var strings = "yay";
for(i in strings) console.log(i + ":" + strings[i]);

// 0: y
// 1: a
// 2: y
// test: function(){ return "test" }
~~~

The simple solution was to wrap the prototype in a defineProperty function making the enumerable property for test false which keeps it from being displayed in for loops.

~~~
Object.defineProperty(String.prototype, 'test',
{
    value: function(){ return "test" },
    enumerable: false
});
~~~

### For...in is Bad

The interesting part is if you were to get the `length` property of those objects with the new extended property you would still get the correct value.  The whole problem can be avoided by using a simple `for` loop.  When we iterate this way we can't see the extra properties of the `String` object anymore.

~~~
String.prototype.test = function(){ return "test" }

var strings = "yay";
for(var i=0, len=strings.length; i < len; i++) console.log(i + ":" + strings[i]);

// 0: y
// 1: a
// 2: y
~~~


### JSLint Says

It is considered bad practice to use a `for in` loop in general and it should be avoided as much as possible.  There are cases where it can't be avoid for instance if you are iterating though a `JSON` object, but this doesn't happen as often and generally you shouldn't be extending these types.  The guys at [JSLint](http://www.jslint.com/lint.html#forin) even recommend wrapping the body of every `for in` loop with an `if` statement to filter out any unwanted or unexpected behaviour.

~~~
for (name in object) {
    if (object.hasOwnProperty(name)) {
        ....
    }
}
~~~

### Something Else To Be Aware Of

This is another interesting example I came across that gives you some different behaviour between a regular `for` loop and a `for in` loop.  These types of little things can throw you off easily if you don't understand the difference between them so it's important to be aware of these kinds of things.

~~~
var a = [];
a[5] = 5; // Perfectly legal Javascript that resizes array

for (var i=0, len=a.length; i < len; i++) {
    // Iterates over numeric indexes from 0 to 5, as everyone expects
}

for (var x in a) {
    // Shows only the explicitly set index of "5", and ignores 0-4
}
~~~

There is also a great [discussion on stackoverflow](http://stackoverflow.com/questions/500504/javascript-for-in-with-arrays) about this topic that you can read for more information.  It's also interesting to take a look at [JSLint](http://www.jslint.com) and to begin testing some of your code there, it will give you some great insights into JavaScript and it's inner workings.