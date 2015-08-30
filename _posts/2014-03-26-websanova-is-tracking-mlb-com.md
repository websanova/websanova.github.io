---
layout: master
title: Websanova is Tracking MLB.com
keywords: mlb, tracking, websanova
description: After seeing a little spike in traffic on Analytics I did some further investigation. Lo and behold Websanova is tracking MLB.com!
date: Mar 26 2014
permalink: /blog/articles/websanova-is-tracking-mlb-com.html
---

I just checked my analytics tracking this afternoon with a pleasant surprise to see a nice little spike in my traffic. I don’t get all that much traffic so even a little spike is enough to get me excited. I have been writing some articles on Node.js lately but nothing too fancy nor have I focused on any kind of promotion or SEO for these articles. As I begin to investigate the source of the traffic I noticed some odd domains appearing in my stats.

Lo and behold it’s **MLB.com!**

![MLB Analytics](/img/analytics-screenie.png)

You can crack open their site here at:

[http://mlb.mlb.com/mlb/fantasy/bts/y2014/splash_index.jsp](http://mlb.mlb.com/mlb/fantasy/bts/y2014/splash_index.jsp)

and search for my Google analytics tracking code `UA-12380236-11`.

![Analytics Source Code](/img/analytics-screenie-2.png)

**But wait… THERE’S MORE!**

Check out that addthis code for social tracking `ra-504fdeb9241970a8`. That’s my id too!

Well I feel flattered the rockstar programmers at MLB would want to copy and paste the code from my blog but why on earth someone would want to copy and paste my analytics and addthis code is beyond me. Too bad they didn’t copy my adsense codes, that would have been a nicer surprise.

I only wish I was there to see their faces as they try to figure out whey they have a big goose egg in their traffic stats in the following days. I wanted to send them an email but now I’m just curious to see how long this might go on for.

## UPDATE:

Well after a bit of sleuthing, the mystery is solved. I noticed they have a scratch card at the bottom of the page. I had written a “scratch pad” plugin so I did a quick search for `wScratchPad` and sure enough it’s there. They must have copied the source code from my [wScratchPad plugin site](http://wscratchpad.websanova.com), div’s, CSS and all attempting to modify it. You can even see the same `id` and `class` names. I guess they copied a little too much.

Kind of feel like a bit of a jackass now, ah well.

## UPDATE 2

For anyone keeping track you can see some of the numbers for the whole debacle in my [MLB.com Part Deux](http://www.websanova.com/blog/articles/what-happens-when-mlb-com-uses-your-google-analytics-code-part-deux) article.