---
layout: master
title: jQuery .clear() – Clearing Element Contents Using a Timer
keywords: jquery, clear, timer, plugin, javascript
description: This is a handy little method to have laying around. It allows you to populate an element and clear it after a certain amount of time.
date: Nov 13 2012
permalink: /blog/jquery/jquery-clear-clearing-element-contents-using-a-timer
---

This is a handy little method to have laying around. It allows you to populate an element and clear it after a certain amount of time. You simply provide a string as a message and optionally provide a `delay` and `fadeOut` duration for the element. The reason I wrote myself this little method is that simply using the jQuery `delay` method didn’t really produce the desired results as it did not allow resetting of the message in the element.

For instance take the little sample below:

~~~
$("#elem").show().html("updated successfully").delay(3000).fadeOut(500);
~~~

This will execute, but say the user processes a form again, it will not cancel the original delay and the message will still fade after the original timer which in some cases can result in the message only showing up for a fraction of a second. We need to be able to reset the function anytime it’s called.

Also the use of `stop` will not work here as it’s use is geared towards animations as stated in the [jQuery api docs](http://api.jquery.com/delay)

We are going to need something a little more robust and quicker to use, so why not just build it all into one function and create a shortcut for ourselves by just doing something like:

~~~
$("#elem").clear("successfully updated"); // or
$("#elem").clear("successfully updated", 3000, 500);
~~~

This is much easier to use and only adds a few lines of code:

~~~
(function($)
{
    $.fn.clear = function(msg, delay, fadeOut)
    {
        return this.each(function()
        {
            var $elem = $(this).html(msg || '').show();

            clearTimeout($elem.data('_clear_timer'));

            $elem.data('_clear_timer',
                setTimeout(function()
                {
                    $elem.fadeOut(fadeOut || 500);
                },
                (delay || 3000) )
            );
        });
    };
})(jQuery);
~~~