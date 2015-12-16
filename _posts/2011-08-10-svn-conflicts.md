---
layout: master
title: SVN Conflicts
keywords: svn, subversion, conflicts
description: Article covers reading and handling conflicts in svn (subversion).
date: Aug 10 2011
permalink: /blog/svn/svn-conflicts.html
redirect_from:
  - /svn-conflicts.html
---

I wanted to dedicate a small tutorial to just handling conflicts and merging on SVN as it seems to be a common sticking point for people new to SVN.  A conflict occurs when the same file has been edited by more then one person.  In most cases if you didn't edit the exact same lines your updates and commits will go through smoothly and merges on the files will happen automatically.  Occasionally there will be conflicts that need to be resolved manually.  

### Creating a Conflict
	
Lets start off by assuming we have a file called `test4.php`, if we look at the contents we will see this inside:

~~~
$ more test4.php
This is line one
~~~

Now lets say one of your co-workers modifies the file and commits it so that it looks like this:

~~~
$ more test4.php
This is one line
~~~

Now lets say you also change the file so that your version looks like this:

~~~
$ more test4.php
This is the second line
~~~

### Seeing the Conflict

You will get a conflict warning one of two ways, when you are trying to commit or you are trying to update.  Lets say you are trying to commit a file and you get an error message like so:

~~~
$ svn commit -m "test"
Sending        test4.php
svn: Commit failed (details follow):
svn: File or directory 'test4.php' is out of date; try updating
svn: resource out of date; try updating
~~~

Subversion is telling us there is a conflict that it can't resolve and that it requires manual intervention.  If you update the file you will see that SVN is telling us there is a conflict on that file indicated by the `C`.  Files that were automatically merged will be displayed with a `G` beside them which stands for merged.  Also you will be prompted with a message with a bunch of options in case you want to use editing tools to fix the conflict, just select `p`.  If you don't get the warning, don't worry about it.

~~~
$ svn update
Select: (p) postpone, (df) diff-full, (e) edit,
        (h) help for more options: p
C    test4.php
G    test5.php
Updated to revision 497.
~~~

So what happens now is that subversion will create a list of four files that look like the following:

~~~
$ more test4.php.r496
This is line one

$ more test4.php.r497
This is one line

$ more test4.php.mine 
This is the second line

$ more test4.php
<<<<<<< .mine
This is the second line
=======
This is one line
>>>>>>> .r497
~~~

- test4.php.r496 - The original version before you made any changes to it.
- test4.php.r497 - The latest most up to date version from your co-workers.
- test4.php.mine - Your version.
- test4.php - The combined version of your changes and your co-workers changes with markers showing where there were conflicts.

### Solving the Conflict

So there are basically three things you can do at this point.  One is that you can scrap your changes and just go with the latest file in the repository.  This is probably your most unlikely scenario, but the easiest to resolve.

~~~
svn revert test4.php
Reverted 'test4.php'
~~~

You can also run the following if it makes more sense to you:

~~~
$ svn resolve --accept theirs-full test4.php
Resolved conflicted state of 'test4.php'
~~~

A second option and also more unlikely is getting rid of your co-workers work and replacing it with your own work.

~~~
$ cp test4.php.mine test4.php
$ svn resolved test4.php
Resolved conflicted state of 'test4.php'
~~~

Here you could also just run:	

~~~
$ svn resolve --accept mine-full test4.php
Resolved conflicted state of 'test4.php'
~~~

The last option is to merge both files and manually resolve the conflict by removing all markers and putting the file into the state you want.

~~~
$ svn resolved test4.php
Resolved conflicted state of 'test4.php'
~~~

Likewise you could also run:

~~~
$ svn resolve --accept working test4.php
Resolved conflicted state of 'test4.php'
~~~

Basically you can see that as long as you run the resolved command SVN will assume everything is okay and set the file as modified at which point you will need to commit the file with whatever route you decided to take.

~~~
$ svn status
M     test4.php
~~~