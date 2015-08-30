---
layout: default
title: Creating Awesome Effects Using jQuery Hover
keywords: effect, jquery, hover
description: An article covering how to use the jQuery hover function to create awesome effects.
date: Feb 20 2011
---

For a recent plugin color picker plugin that I wrote I was setting up some slide and fade effects for a palette that would open and close.  I initially used the simple mouseover/mouseout combo but quickly realized the ineffectiveness of this method.  I was saved by using the jQuery hover method, a great little method that solved all my problems and then some with just a few lines of code.

I thought I would outline it here as it's a great way to get really smooth effects like fades and slides that look fantastic without any jerkiness.  I also wanted to outline why the other solutions didn't work, so the following is my work flow on how I came to the solution I got to.

For a sample of this effect in action check out my color picker plugin here: [Websanova jQuery Color Picker Plugin](http://wcolorpicker.websanova.com)

For the purpose of this workflow assume we have a button that when moused over would display a palette directly below it.

### Using mouseover and mouseout

This can work for simple situations but creates all sorts of issues with the event bubbling if you have other elements within you main event element, not to mention those inner elements having their own events.  For instance the mouseout for your main element will be triggered if you mouseover an inner element. Although there may be situations that call for this, for the most part unless we actually mouseout of our main element entirely we probably don't want to trigger anything within the main element.  The first thought was to create some flags to track the mouse events, but that can get messy fast.

~~~
buttonHolder
.mouseover(function(e){$this.showPalette($this);})
.mouseout(function(e){$this.hidePalette($this);})
~~~

### Using hover()

Switching to hover, solves these problems instantly and was a life saver.  It takes two arguments, the first for mouseover and the second for mouseout.  We basically take our existing code, pop it in and it takes care of all the hover nonsense.

~~~
buttonHolder.hover(
    function(e){$this.showPalette($this);},
    function(e){$this.hidePalette($this);}
)
~~~

###Using stop() to Clear the Animation Queue

Now we have some proper hover on/off functionality going, but our problems are not all gone.  If we were to hover on/off quickly two or more times, each hover event will run in succession.  Again we are faced with using flags, but thankfully jQuery has already taken care of that for us with the use of the stop() method.  Basically any animation effects we set are queued and will run one after the other, using stop lets us clear this queue, it takes two arguments that we will look at.

~~~
.stop( [clearQueue] [, jumpToEnd] )
~~~

The two arguments for stop(), `clearQueue` and `jumpToEnd` give you a lot of control over how your animations will work and are important to know about.  The first parameter allows you to clear the current queue and run the new animation.  The second parameter lets you say whether you should continue running the current animation or stop that as well.

### Using stop(true, false)

In our particular situation we will set the parameters to true and false before calling our next animation, also we need to make sure that the stop method is the first thing we call.

~~~
$this.paletteHolder.stop(true, false).css({height: 0}).show()
.animate({height: $this.colorPicker.outerHeight()}, $this.settings.showSpeed);
~~~

We set the first parameter to true to make sure the queue is cleared and we set the second parameter to false to stop the current animation as is and continue with the next one.  This is where we get some really cool effects as in the case of a drop down where we hover on/off, our slide will stop half way then revert if we were to hover on/off quickly.

### Converting fadeIn()/fadeOut() to Animations

Our hover effect is pretty much running smoothly now, but we will have to convert some of our effects to animations.  For instance I was using the fadeIn() and fadeOut() methods in the case of a fade effect for the palette.  This can be easily converted to an animation with the following code below.

~~~
$this.paletteHolder.stop(true, false).show()
.animate({opacity: 1}, $this.settings.showSpeed);

$this.paletteHolder.stop(true, false)
.animate({opacity: 0}, $this.settings.hideSpeed, function(){$this.paletteHolder.hide()});
~~~

Note that we need to run the show/hide calls here since opacity 0 is not really a hidden div, and is still technically visible which will overlay elements on your page.  I then substituted the fades by using the opacity value and then moving towards 0 or 1.

### Using "active" to Avoid Multiple Hides

This is required if you have an event within your main element that triggers the hide, for instance a click on a color in the palette would run the hide function, which would run again on the mouseout from our original hover function.  This can create some unwanted results, by simple putting the check in it ensures it's only run once.

~~~
showPalette: function($this)
{
    $this.paletteHolder.addClass('active')

    //run some code
},

hidePalette: function($this)
{
    if($this.paletteHolder.hasClass('active'))
    {
        $this.paletteHolder.removeClass('active');
        
        //run some code
    }
},
~~~

### Putting it All Together

Putting it all together you will get something that looks like the code below.  In the end we accomplish quite a lot with only a very small amount of code once we realize the problems we have had to solve.

~~~
Holder.hover(
    function(e){$this.showPalette($this);},
    function(e){$this.hidePalette($this);}
)

showPalette: function($this)
{
    $this.paletteHolder.addClass('active')
    
    if($this.settings.effect == 'slide')
    {
        $this.paletteHolder.stop(true, false).css({height: 0}).show()
        .animate({height: $this.colorPicker.outerHeight()}, $this.settings.showSpeed);
    }
    else if($this.settings.effect == 'fade')
    {
        $this.paletteHolder.stop(true, false).show()
        .animate({opacity: 1}, $this.settings.showSpeed);
    }
    else
    {
        $this.paletteHolder.show();
    }
},

hidePalette: function($this)
{
    if($this.paletteHolder.hasClass('active'))
    {
        $this.paletteHolder.removeClass('active');
        
        if($this.settings.effect == 'slide')
        {
            $this.paletteHolder.stop(true, false)
            .animate({height: 0}, $this.settings.hideSpeed, function(){$this.paletteHolder.hide()});
        }
        else if($this.settings.effect == 'fade')
        {
            $this.paletteHolder.stop(true, false)
            .animate({opacity: 0}, $this.settings.hideSpeed, function(){$this.paletteHolder.hide()});
        }
        else
        {
            $this.paletteHolder.hide();
        }
    }
}
~~~

This gives us a default no effect, a slide effect and hover effect with a ton of intelligence and some really great looking effects.