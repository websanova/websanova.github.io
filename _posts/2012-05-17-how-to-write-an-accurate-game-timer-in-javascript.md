---
layout: master
title: How to Write an Accurate Game Timer in JavaScript
keywords: javascript, game, timer
description: Article about how to implement an accurate FPS game timer in JavaScript.
date: May 17 2012
permalink: /blog/javascript/how-to-write-an-accurate-game-timer-in-javascript.html
redirect_from:
  - /tutorials/javascript/how-to-write-an-accurate-game-timer-in-javascript.html
  - /how-to-write-an-accurate-game-timer-in-javascript.html
  - /blog/javascript/how-to-write-an-accurate-game-timer-in-javascript/feed.html
---

This week I decided to revisit a game I had made a long time ago using HTML5 canvas called [Blocknik](http://blocknik.websanova.com), a Tetris style game I made to learn some HTML5 and how to use canvas.  A buddy of mine wanted to get it into the app store for the iPad so I did a little overhaul of it, specifically with the timer.

Originally when I had written it I used what made the most sense to me at the time, basically you set your frame rate (30fps), divide that by 1000 milliseconds for one second and you get your interval between game loops, approximately 33.33 milliseconds.

The run code looked something like this:

~~~
function loop()
{
    var start = (new Date).getTime(); // get the start time
    run_game_code();
    var end = (new Date).getTime(); // get end time
	
    var delta = end - start; // gets how long the game code ran for
	
    var delay = 33.33 - delta // 33.33 is the total delay between loops
	
    setTimeout(function(){loop()}, delay); // delay the difference to get 33.33 intervals
}
~~~

This seems very intuitive but this never worked consistently between browsers, they were always slightly off, and I had to make adjustments for each browser individually.  What happens is that we are only taking the time difference for the game loop, but not for the other JavaScript code that is running and this gives us that slight variation, although small it will be particularly noticeable in Chrome.

What we need to do is to find a way to calculate the time in only one spot, preferably after the game code and before the `setTimeout()`.  The solution is quite simple and actually requires less code than the one above.  The following code below is a little Timer object I created that will give us consistent timing in all browsers, explanation follows after the code.

~~~
function Timer(settings)
{
    this.settings = settings;
    this.timer = null;

    this.fps = settings.fps || 30;
    this.interval = Math.floor(1000/30);
    this.timeInit = null;
		
    return this;
}

Timer.prototype = 
{	
    run: function()
    {
        var $this = this;
		
        this.settings.run();
        this.timeInit += this.interval;

        this.timer = setTimeout(
            function(){$this.run()}, 
            this.timeInit - (new Date).getTime()
        );
    },
	
    start: function()
    {
        if(this.timer == null)
        {
            this.timeInit = (new Date).getTime();
            this.run();
        }
    },
	
    stop: function()
    {
        clearTimeout(this.timer);
        this.timer = null;
    }
}
~~~

The only thing we know is consistent between the browsers is the time, everything else we will assume can run slightly differently based on OS and JavaScript Engine.

- Get our game loop interval time.  I used the floor function on this to avoid any decimal or rounding issues that may occur between browsers.
- Get the start time only once before we start the game loop and make sure the timer is not already running.
- After each run of the game loop we increment our <i>initTime</i> by the game interval time manually.  This avoids the use of any browser date functions and sets our "target" time for each interval discreetly.
- We run our date function once and then subtract it from the <i>initTime</i>.
- We don't have to worry about checking for a lower bound because the <i>setTimeout</i> function already has a minimum time that it runs it's intervals at.
- This greatly minimizes any fluctuation in our time calculations between browsers.  Pretty much the only thing we're missing is the subtraction operation between <i>timeInit</i> and the <i>getTime</i> call, this should pretty much be negligible.

We then just run our game code like so:

~~~
var timer = new Timer({
    fps: 30,
    run: function(){
        //run game code here
    }
});

timer.start();
timer.stop();
~~~

So there you have it perfectly synched time in all browsers.  This is probably about as accurate as you can get and it adds a very tiny foot print to your game loops.