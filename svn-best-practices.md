---
layout: default
title: SVN Best Practices
keywords: best, practice, svn, subversion
description: Article covers some of the best practices with SVN (subversion) via the command line.
date: Jul 27 2011
---

This section outlines some of the best practices in how your repositories should be structured.  There are three main sections repositories usually follow and they are called the trunk, tags, and branches.  You are completely free to structure your repository in anyway you see fit, however these are the more common industry standards and terminology in use and it is generally a good idea to follow and be familiar with them.

### Multiple repositories and projects.

The base of your repository should typically reflect the different projects you are working on but I find it is really dependent on complexity.  For my personal repositories where I have multiple small projects going on at a time, I may set up multiple roots like so:

~~~
www.duchnik.com
    /trunk
    /tags
    /branches

www.pixanova.com
    /trunk
    /tags
    /branches
~~~

However in a company environment where you have much larger projects and many brands its usually a good idea to create separate repositories for each major project.  You will however see this implemented in different ways, where a particular brand within a company may have multiple sub folders representing different portions of each brand or products.

#### Repository 1 (duchnik.com):</b>	

~~~
www.duchnik.com
    /trunk
    /tags
    /branches

DownloadManager
    /trunk
    /tags
    /branches
~~~

#### Repository 2 (websanova):</b>

~~~
www.websanova.com
    /trunk
    /tags
    /branches

platform.websanova.com
    /trunk
    /tags
    /branches
~~~

However many repositories you create or however many projects you decide to setup per repository the key is to always make sure that you have the trunk/tags/branches structure within each project.

### Trunk

The trunk is where the core of your work will happen and it represents the most current version of your code even if it contains bugs.  You will typically develop your features in the trunk and then commit them once they are complete into the central repository.  When you're working on a new version of your code, you will typically get your trunk to a state where its ready to be versioned off as the next latest and greatest version of your code.  Sometimes though, you will see the trunk versioned off even if there is no official release, and it is rather to jut take a snap shot of the code base at that current state, maybe for branching or split testing.

### Tags

Tags are where you will be maintaining all the versions of your code.  I find the most basic structure is a three number versioning system, where the first two numbers are an external representation of the version of that code, while the third number is more of an internal representation just representing bug fixes.  So for example, lets say you being your project and you release your first version and you tagged it as `version_1.0.0`.

~~~
www.duchnik.com
    /trunk
    /tags
        /version_1.0.0
~~~

At this point typically your trunk and version_1.0.0 will be exact copies of each other.  Now you continue developing and adding new features, you eventually decide you want to release a new version which includes some awesome new features.  You then would tag it again, but this time increment the version number like so:

~~~
www.duchnik.com
    /trunk
    /tags
        /version_1.0.0
        /version_1.1.0
~~~

You will then get to a point where, you want to completely revamp or upgrade your application in a way that breaks from the current version line.  There is no hard rule for this and is really just based on how you feel your application is evolving, but you may make a major version update like so:

~~~
www.duchnik.com
    /trunk
    /tags
        /version_1.0.0
        /version_1.1.0
        /version_2.0.0
~~~	

You can continue to add versions in this fashion, allowing you to keep a clean history of each version you have ever released.  What happens is next is that you may find a bug in your code and you will want to fix it and merge it back in.  There are a few ways to do this, the most common is usually to just go into that tag and fix that bug then increment the version number, however, you can also make a copy in your branch, fix the bug there and merge those changes up through the different versions.  However merging in SVN can be a pain sometimes and in smaller environments you will usually see the former approach used.  However you decide to get your bug fix into each version that needs it, at this point you will increment that final version number.  This again can be done in different ways, with some people preferring to just rename the current folder to that latest version and others preferring to create a completely.  I typically prefer to rename the folder as I don't really care about the previous version with the bug, I just want the latest most bug free version.

~~~
www.duchnik.com
    /trunk
    /tags
        /version_1.0.0
        /version_1.1.0
        /version_2.0.1
~~~ 

### Branches

Branches are typically used to fix bugs and to develop features outside of the trunk.  Sometimes you will be doing your regular everyday development in the trunk and you will get a request to add some feature or fix a bug.  The problem is you can't just update to another version of the code in your trunk because you will lose your current code and you don't want to commit the code you currently have because its not finished yet.  The trick is to make a copy from one of the versions under tags, and copy into branches with a name like `feature_4_download-manager`.  From there you can develop the feature independently and then merge into any other tags and into the trunk later on.  This allows you to have a clean version of that code containing only that feature or bug, which you can then decide what other versions should include it.

~~~
www.duchnik.com
    /trunk
    /tags
    /branches
        /feature_4_download-manager
        /feature_7_icon-gallery
        /bug_46_invalid-character
~~~

From the above example you can see how this might be structured with multiple features.  I've also showed a bug here too and this follows the same ideas as adding a new feature.  In most cases you will also see a number associated with the path, which usually represents a case or bug number in whatever project management application you are using.  This gives you the number which you can look up directly and find out exactly what that bug or feature is supposed to fix or add.  The small two to three word description is there just as a quick reference.