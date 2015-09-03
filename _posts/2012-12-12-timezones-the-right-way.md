---
layout: master
title: Timezones, the Right Way
keywords: mysql, websanova, timzones
description: Covers handling timezones in MySql
date: Dec 12 2012
permalink: /blog/php/timezones-the-right-way
---

This is a little article on how to deal with timezones when storing dates/times in your database. The database or language you choose to use really doesn’t matter as the concepts are universal but I will be giving examples using MySQL and PHP. Timezones are actually a very trivial concept but they seem to be overlooked and over complicated. Typically when you setup a database or are using some web hosting service there will be some default timezones set. This default is okay if you have one server, but what happens if you decided to move your server or have multiple servers in different locations?

Rather than storing a timezone with each date it’s better to just accept a standard time to store all your dates with, thus doing the conversion to that standard time before storing the value in the database. It doesn’t really matter what time we store it as, but it’s generally a good idea to just use `UTC+00:00`.

## What is UTC

UTC stands for "Universal Time, Coordinated" and is the successor to Greenwich Mean Time (GMT). They are almost synonymous with each other but UTC is considered the accepted standard. Basically, UTC is just a division of time with 40 distinct time zones. It has a 00:00 point with plus and minus 12 major timezones and also some additional half timezones like `UTC+04:30`. The idea is that if you have a timezone of `UTC-05:00` then you would have to subtract -5 hours from the `UTC+00:00` time to get to that time. Meaning if it’s 14:00 (or 2pm) in your timezone at `UTC-05:00` (representing EST or Eastern timezone) then `UTC+00:00` would be 19:00 (or 7pm).

[Graphical Representation of Worlds Timezones](http://upload.wikimedia.org/wikipedia/commons/8/88/World_Time_Zones_Map.png) (Courtesy Wikipedia)
[List of Timezone Abbreviations](http://en.wikipedia.org/wiki/List_of_time_zone_abbreviations) (Courtesy Wikipedia)

## Storing Time as UTC+00:00

The first thing we would need to do is decide what timezone to store all our times in. I highly recommend using `UTC+00:00` as your base timezone for no particular reason other than it’s easy to remember and 0 just represents a nice starting point. Once we know all the times are in `UTC+00:00` then it becomes much easier to represent the proper time for each user based on their own personal timezone. Once a user has selected (or we have auto-detected) their timezone, then displaying the proper times for that user becomes trivial.

## Setting Default Timezone

Assuming all our dates/times are stored in UTC format we can simply set the default timezone that will be used to display all the times. This will automatically adjust for all the date and times given. In PHP all we do is set the timezone like so:

~~~
date_default_timezone_set('EST');
~~~

We can put this in some kind of init at the beginning of our code and we’re done, all times will be displayed properly for that users timezone now.

Alternatively if we really wanted fine grained control we could do it just for a specific date and build a function like this:

~~~
function get_date($date)
{
    date_default_timezone_set('EST');
    $date = date("Y-m-d H:i:s", strtotime($date));
    date_default_timezone_set('UTC');

    return $date;
}
~~~

## Configuring PHP for UTC+00:00

Of course all would be for not if we didn’t at least configure our system for UTC as the default timezone. I’m sure this can be done similarly in other languages, but in PHP we do it like so:

~~~
date.timezone = UTC
~~~

Now whenever we use things like the `time()` function this will automatically give us the time in UTC with the proper offset based on our servers time. If we wanted to set dates in a database like MySQL we could then set the values as:

~~~
date("Y-m-d H:i:s", time());
~~~

If we didn’t have access to change the PHP configuration, then we would have to adjust the time manually when setting dates which could be done like so:

~~~
date("Y-m-d H:i:s", time() - date("Z"));
~~~

## Configuring MySQL for UTC+00:00

Setting the dates directly as we did above is useful if we don’t have access to change the MySQL configuration parameters. However, if we want to use things like `TIMESTAMP` which will typically update themselves or MySQL date functions like `NOW()`, then we would have to set MySQL to use UTC as a default timezone as well.

In MySQL this is done by setting the UTC offset which can be done just as easily as it was in PHP by setting the following:

~~~
default-time-zone = '+00:00'
~~~

## Updating Existing Dates/Times

There is a good chance that if you decided to standardize your timezones using UTC with a default `UTC+00:00` offset that now your existing times in your database will be off.

Thankfully MySQL provides a handy function to let us easily update our existing timezones.

**NOTE:** If you have any timestamps that auto update, you will want to do them first.

We can search for all date and time fields in our schema with the following query:

~~~
SELECT * 
FROM `information_schema`.`COLUMNS` 
WHERE 
   `TABLE_SCHEMA`='table_name' AND 
   (`DATA_TYPE`='timestamp' OR `DATA_TYPE`='datetime' OR `DATA_TYPE`='date');
~~~

From here we can use the following query to update the dates/times in our tables:

~~~
UPDATE `table_name` SET `timestamp`=CONVERT_TZ(`timestamp`, '-05:00', '+00:00');
~~~

Where `-05:00` would be the current timezone that the time is currently stored as, which in the above case would be Eastern. The `+00:00` is the timezone we want to convert it to which in this case is UTC.