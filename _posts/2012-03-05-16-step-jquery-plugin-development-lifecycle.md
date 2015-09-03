---
layout: master
title: 16 Step jQuery Plugin Development Lifecycle
keywords: jquery, plugin, lifecycle
description: Article outlines 16 steps involved in th jQuery plugin development lifecycle.
date: Mar 5 2012
permalink: /blog/jquery/16-step-jquery-plugin-development-lifecycle
redirect_from: /tutorials/jquery/16-step-jquery-plugin-development-lifecycle.html
---

I'm always looking for ways to improve my development whether it is through coding or through process.  The main reason for this list is to have a specific set of instructions in order to avoid duplication of process.  Many times I will launch a plugin only to find some more bugs, so this list was the result of an attempt to avoid repetition and ensure my plugins launch as bug free as possible.

I have written this specifically for jQuery plugin development, however I'm sure something similar could be followed in other cases.  The guide focuses strictly on the development process and does not include things like the concept phase or bug reporting, it's assumed we have some set piece of code in mind that needs to be developed. 

It may seem like a lot of steps, but I have broken them down very discretely as I find they can be missed easily.  I have broken it down into four major steps: develop, package, document and release. 


### Develop - Coding

We have something in the queue to work on, so quite simply it needs to be coded, this can be a bug, a feature, or just an update to the code, an optimization for instance.

### Develop - Commit

I like to use some kind of versioning system, there are plenty of great services out there like Google Code and GitHub that are great for this.  I don't want to get into the details of how you should maintaining your repository as that would be a lengthy article on it's own, but suffice it to say that you should try to commit all your changes separately rather then making 3 fixes an update and adding a new feature and committing that in a bundle.  This is what is great with distributed version control systems as the commits happen locally and then you push your commits to a main server.  Doing things this way allows you to see the history of all your changes much better, pin point a specific version and avoids conflicts if you're working with a larger team.

### Develop - Push

This case probably only matters if you are using a distributed versioning control system, but whether you're working with a team or you're just running an open source project eventually you will need to push your code.  This is a phase where your code will need to be combined all into one.

### Develop - Release Candidate

Eventually you will get to a point where you will have a release candidate for the public, at this step you either move forward to the release phase or return to step 1 for further development.

### Package - Separate Files

At this point I will take all the files for the plugin and begin to package them by putting them in a temp folder to make sure I don't mess with my repository files by accident.  If you have a bigger project this can be done with an export from your version control system.

### Package - Minify / Obfuscate

Create minified version of necessary files.  This is a simple step that takes about 2 minutes and is just nice to do for developers.  In a collective thought, it is better that only one person has to do this rather than everyone who downloads the plugin.

### Package - Update Version in File Header

Update version in comment header of each file.  I always forget to to do this, so I made it a single step here.  Note that in my repository version of the file I just have x.x.

~~~
 * @version         Version 1.5
~~~

### Package - Update Version in File Name

Update file names to include version number.  This is not a required step, but I always prefer to know what version of a plugin I'm using without having to dig around.  I always name my files something like `wTooltip.1.5.js` or `wTooltip.1.5.min.js`

~~~
./wTooltip.1.5.js
./wTooltip.1.5.min.js
./wTooltip.1.5.css
~~~

### Package - Update Demo

I always like to include a demo html file with my plugin that contains some examples, in my case it's always the index.html.  Make sure you are using the correct version of the files in your demo .html, then TEST the html file by just tossing it into a browser.

### Package - Archive

Finally zip / archive the files.  I like to name my archive after the version number as well so you will see something like:

~~~
./wPaint.1.2.zip	
~~~

### Document - Site Docs

I always have to force myself to do this one as there is a tendency to kind of skip it or put it off for "later".  Updating your docs only takes a few minutes in most cases and although it's a total chore it's something that should be done regularly for others to understand what is going on with your code.

Samples of site docs here: [jQuery Tooltip](http://wtooltip.websanova.com), [jQuery Color Picker](http://wcolorpicker.websanova.com)

### Document - Site Demo

Now before I upload my zip file or do any tagging, I take the zip file and put it into the demo page on my site as if I were a user.  This is a step that lets me check if I forgot anything or if there are any other last bugs I missed.  This is really the make or break point, if everything here is okay, we launch otherwise we go back to step one.

### Release - Tag

If all is good I will tag the plugin in my version control system at the version number and call it official.

### Release - Push

One final push to push all tags that were just created.

### Release - Upload

We can also upload our archive right away here for publicly available download.

### Release - Social

One last step at least for me is to login into my Twitter account and post a tweet.  You should update anything you can at this point, newsletters, Facebook, anything that lets user's know you have a new version of the plugin.

That pretty much covers the life cycle of jQuery plugin development.  These are steps I have followed for a login time and although they seem simple and obvious on the surface have taken me a while to fine tune to a point of optimal efficiency.  This is just another piece of the puzzle I don't need to think about anymore allowing me to free up my brain for other more important purposes.