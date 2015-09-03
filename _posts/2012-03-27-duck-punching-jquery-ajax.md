---
layout: master
title: Duck Punching jQuery Ajax
keywords: duck, punching, jquery, ajax
description: An article describing the technique of duck punching with an example using jQuery $.ajax function.
date: Mar 27 2012
permalink: /blog/jquery/duck-punching-jquery-ajax.html
redirect_from: /tutorials/jquery/duck-punching-jquery-ajax.html
---

I recently came across a very particular problem using jQuery's `ajax()` function.  I wanted to have a global handler that would intercept all "success" requests, check them for a return code and only continue processing if the code returned had a value of "1".  It would then display a graceful message regardless the value of the return code.  I also did not want to add any extra code into each success function as I have about 100 different ajax requests.  The existing jQuery ajax utilities were unable to accomplish this so I needed a different solution.

The solution is a design pattern referred to as "duck punching", and it has three parts:

- save the old function
- overwrite the function with your own function
- insert the old function back into your new function somewhere

Note that we are not dealing with ajax errors themselves, we are assuming the request was sent okay, but that perhaps input was incorrect, or an email already exists, something of that nature.

The most trivial example is quite simple and looks something like this:

~~~
var _old = someFunc;

someFunc = function(arg1, arg2)
{
    //add some code here
    
    return _old.apply(this, arguments);
}
~~~

This is a fantastic JavaScript technique that is definitely good to have in your bag of tricks.  What's great is that we can add some conditionals and completely avoid running the old function altogether if we wanted to:

~~~
var _old = someFunc;

someFunc = function(arg1, arg2)
{
    //add some code here
    
    if( [condition] )
        return _old.apply(this, arguments);
    else
        // do nothing
}
~~~

This now fulfills the criteria for our initial problem and lets us hijack the jQuery `ajax()` `success` callback and insert some of our own code.  We will take advantage of the ajaxPrefilter() function here which lets us modify our ajax options before they are sent.  Note how we also make sure the success callback is set so that we don't interfere with some of the other shortcut functions in jQuery like load() and getScript().

~~~
$.ajaxPrefilter(function( options, originalOptions, jqXHR )
{
    if(options.success)
    {
        var _old = options.success;

        options.success = function(response)
        {
            ajaxPostProcess(response);
            if(response.code &gt; 0) _old.apply(this, arguments);
        }
    }
});
~~~

Let's say we always return JSON data in a format like follows:

~~~
{
    "code": "1",
    "message": "Email already exists",
    "data": "data here if any"
}
~~~

Now when we make any ajax calls we don't have to worry about handling any response from the server, we know it has passed our tests and we can focus on just writing the "success" logic for that particular ajax call.