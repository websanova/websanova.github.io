---
layout: master
title: SVN Command Reference
keywords: svn, subversion, command, reference
description: Article covers list of common svn (subversion) commands along with how and when to apply them.
date: Aug 2 2011
permalink: /blog/svn/svn-command-reference.html
redirect_from:
  - /blog/svn/svn-command-reference/feed.html
---

Rather then just give a list of command and what they do, in the section I wanted to cover some real world scenarios that you will come across and how to handle them.  You will see the specific commands to use, and learn what they do as you apply them.

### Summary of Commands

This is a brief overview of the commands that you will learn about in this tutorial.

~~~
svnadmin create #create a new repository
svn info        #lists the information for the repository
svn mkdir       #make a directory in the repository
svn checkout    #gets a copy of the repository and puts it under version control
svn export      #get something from repository (will not be under version control)
svn import      #put something into repository
svn commit      #find all changes and put them into the repository
svn status      #shows a list of changes
svn status -u   #shows a list of changes in the repository that don't exist in my copy
svn add         #mark a file to be added to the repository
svn move        #move something in the repository from one spot to another
svn delete      #delete something from the repository
svn rename      #basically a move and a delete done in one step
svn copy        #copy something in the repository
svn log         #shows a list of commit messages
svn list        #display contents of repository directory
svn update      #update my checked out copy to the whatever is in the repository
svn update -r   #update to a specific version of the repository
svn diff        #check difference between files
~~~

### Creating SVN Repository

You can create as may of these as you want, and setup your repositories to point to these locations [Installing and Configuring SVN on CentOS](/install-and-configure-svn-on-centos), from there you will be basing all of your other commands off the root of this location.

~~~
svnadmin create /path/to/<repository_name>
~~~

### Importing New Code to Repository

A typical scenario is that you will need to import code that is not in the repository yet and make it an active `svn` code base.  There are two ways to accomplish this so I will cover them both, however there are some preliminary steps we will need to do for either situation first.

First we are going to create a folder for the new project we are importing and also create some of our initial directories [SVN Best Practices](/svn-best-practices) with the use of the `mkdir` command.  The `-m` option is just to specify a `message`.  Anytime you make a change to the repository you will have to specify a message even if you want to leave it blank.  Note that the directories are being created directly on the repository.	

~~~
svn mkdir http://<domain>/newProject -m "Creating newProject"
svn mkdir http://<domain>/newProject/trunk -m "Creating trunk folder"
svn mkdir http://<domain>/newProject/tags -m "Creating tags folder"
svn mkdir http://<domain>/newProject/branches -m "Creating branches folder" 
~~~

The first method is much faster and assumes that you just want to make whatever your directory with your code is an active SVN code base under version control.  In this case we simply checkout the empty project we just created into that directory.  In this case we will be checking out the trunk directory, since that is where we want our active code development to be.

~~~
svn checkout http://<domain>/newProject/trunk /path/to/code
~~~

This basically links our directory with all of our code to the SVN repository and tells it to start keeping it under version control.  If you run the status command we will see that all the files and folders have a question mark beside them which is basically just SVN telling us they are all new files, and it wants to know what to do with them.

~~~
svn status /path/to/code
~~~

We need to tell SVN what to do with these files so we will just tell them to add them to the repository by running the following command:

~~~
svn add /path/to/code/*
~~~

You will see some output as it goes through each file and directory adding files.  When its done and you do a status check again you will this time see the letter `A` beside each file telling you that these files are ready to be added to the repository.

~~~
svn status /path/to/code
~~~

Now we can do our initial commit to put all these files into the repository and they will now be under version control.  Since we are making a change to the repository again here we will need to specify our -m option with a message.

~~~
svn commit -m "Commiting initial code base to newProject"
~~~

You will see some output here as the code starts being transferred to the repository.  This can take a little while if your project is really large so just be patient and let it finish.  Once its complete we can do our status command again and this time you won't see any output as there are no changes and everything is synced up.

~~~
svn status /path/to/code
~~~

I typically prefer the first method because it allows me to just take my current code and put it under version control without having to create any copies of my code base.  However there are situations where you will not want to put your active code base under version control, or there is just no reason to.  In this case we will just import our code straight into the directories we created earlier.  Again in this situation we would have to provide a message since we are making modifications to the repository.

~~~
svn import /path/to/code http://<domain>/newProject/trunk -m "initial import"    	
~~~

You will see a bunch of output here as the code is being transferred to the repository.  Just hold tight as it could take a while depending on the size of the project.  Once that is done, you can checkout the project wherever you like and that will just be your latest version of that code under version control.

~~~
svn checkout http://<domain>/newProject/trunk /path/to/new/code
~~~

If you do a status check now you will not see any output as you now have the latest version of the project you just checked out in a new directory under version control.

~~~
svn status /path/to/new/code
~~~

Now what you decide to do with that old code base is up to you.  You can continue checkout multiple copies of the code base now as needed, which I will cover in the next section.	

### Repository Information

This is just a smiple command to give you some basic info on your working directory under version control.  Most importantly it tell you the location of the repository of the URL it is connected to.  Sometimes useful if you are managing many code bases.

~~~
svn info
~~~

### Checking Out Code From Repository

Assuming your code is up in a repository somewhere and you just want to grab a copy of it for yourself, all you need to do is run the checkout command.

~~~
svn checkout http://<domain>/newProject/trunk /path/to/code    	
~~~

You could check this out multiple times as long as the directories are named differently, also you could checkout any part of the repository.  In fact you could just checkout the entire code base with all tags and branches although I would not recommend this as the size of the repository can grow pretty fast as you continue to do development.	

~~~
svn checkout http://<domain>/newProject/tags/someVersion /path/to/new/code2		
~~~

### Exporting Code From Repository

Sometimes you will want to get a copy of the code but you wont need to keep it under version control, you simply want the latest version.  You will find this in many cases where you are getting code from some external source, for instance [WebSVN](/setup-up-and-configure-websvn).  In these cases you are not developing the code form this particular source, you just want to use it.

~~~
svn export http://<domain>/newProject/tags/someVersion /path/to/new/code
~~~

### Viewing Contents of Repository

Sometimes we just want to see what is in the repository. There are two things you willy tpically do, the first is to just list the content of a directory like so:

~~~
svn list http://<domain>/newProject/trunk
~~~

The second thing is that you will want to see a log of changes for a particular file or directory.  Sometimes we may also just want to view the log or modifications for a specific version.  Al list of the common different options below:

~~~
svn log /path/to/<file>              #show log for this file
svn log -r 45:77 /path/to/<file>     #show log between the two revisions
svn log -r 77:45 /path/to/<file>     #show log between the two revisions in reverse
svn log -r 23 /path/to/<file>        #show the log for this version
svn log -vr 35 /path/to/<file>       #show modification for this version
~~~

### Updating and Committing New Code

Lets say we are into development now with a few other developers and code is being committed.  What will happen is we will want to commit our own code as well as get other people code.  Typically before I commit, I like to check for updates first, get the latest version of code to handle any conflicts then commit my code.  First we will check incoming changes to our own code.

~~~
svn status -u /path/to/code    	
~~~

This will show us changes in the repository that we don't yet have in our own code base.  We can decide to update only parts of our code by specifying the file or directory:

~~~
svn update /path/to/code/<file>    	
~~~

Or we can just update all of our code in one shot by running update on the base directory.

~~~
svn update /path/to/code
~~~

You will see some output as you code all gets update.  Where it can SVN will automatically merge your code for you, otherwise it will leave conflicts for you to figure out.  Now that we have the latest code and have resolved all our conflicts we can now commit our own code.  First we will just check our own changes by checking our status.  SVN will tell us which files have been changed by putting an "M" beside the file or directory for modified.

~~~
svn status /path/to/code
~~~

Here again, we can just commit a specific file or directory or just the whole code base.	

~~~
svn commit /path/to/code -m "This is my code on...."
~~~

Note that we did not have to do the update first.  We could have just committed our code with out taking anyone elses code.  However its usually a good idea to update first to make sure you code works with the current code in the repository before committing it.  If everyone follows this, you will have much less bugs committed into the repository.

### Checking For Differences

Another very useful command that is not used very often, but very useful when it is required is the "diff" command.  You can run a diff on any file under version control in your working directory and it will list the differences between that file and the file on the repository.  I find I use this command mostly when I'm about to do an update and I just want to see the differences in a file before I change it.  You can run this command on a specific file or just on the directory, which will list differences for all changed files. 

~~~
svn diff /path/to/code
~~~

### Deleteing and Renaming Files

This is a common mistake for developers new to source control with SVN.  You can not just delete a file from the file system as SVN will not know what to do with it.  Similarly with a rename, SVN will see the new file that was added as the renamed version of the file, however the old name of the file will come up as missing to SVN.  If you do this it is not a big deal, but either way you will still have to tell SVN what you did. Resources marked with an exclamation mark, "!" is how SVN tells you something is missing.

~~~
svn delete /path/to/code/<file>	
~~~

If you run this it will remove whatever you are trying to delete from both the repository and file system.  If you do a file system delete, you will still also just need to run the svn delete.  If you check the status you will now see deleted files marked with a `D` for deleted. We now have to do a commit to put the changes through to the SVN repository.

~~~
svn commit -m "Deltetes some file...."	
~~~

In the case of a rename, if we just moved or renamed the file through the file system first, then we will have to do an add for the newly named file.

~~~
svn add /path/to/code/newFile
~~~

Also we will need to do a delete for the old file that no longer exists.

~~~
svn delete /path/to/code/oldFile
~~~

Then we would just run a commit.  However if we just did a rename this would all automatically be done for us.

~~~
svn rename /path/to/code/oldFile /path/to/code/newFile
~~~

if you do a status check here you will see the add and delete being preformed, then we can just commit normally.

~~~
svn commit -m "Rename some file"
~~~

We can also do a delete or rename directly in the repository and then do an update.  This is not very common but occasionally useful.

~~~
svn delete http://<domain>/newProject/trunk/someFile -m "deleteing file"

svn rename http://<domain>/newProject/trunk/old http://<domain>/newProject/tunk/new -m "rename"
~~~

If we then did a status check against the repository we would see the changes that are coming in and we could just do an update.

~~~
svn status -u
svn update
~~~

### Copying Files

At some point you will probably need to branch or create a new tag.  I prefer and highly recommend you do this on the respository when branching or tagging.  Its always best to make a copy of what is in the repository because it assumed it has the latest code.  I have had many instances where someone including myself has made a copy of a directory to tag, but they did not update their local code base first.  This means a somewhat older version of the code was being copied.  This led to some very frustrating issues when launching code to production.  Therefore its best to copy code in the SVN repository when tagging or branching, for the most part at least.	

~~~
svn copy http://<domain>/newProject/trunk http://<domain>/newProject/tags/version_1.0.0 -m "copy"
~~~

### Reverting Changes

Sometimes you will update a change on a file but you will later want to revert it back to an older version.  You can do this using the update command by just specifying the version number you want to revert to.  Usually before I do an update I like to check the status of the file or directory against the repository.

~~~
svn status -u
~~~

Here we will see the current version of the file vs the version of the file in the repository.  We can take a note of this before we do our update, so that if we need to revert we can just run one command to go back to the state of that file for whatever reason that may be.

~~~
svn update -r <version_number> <file>
~~~