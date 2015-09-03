---
layout: master
title: How to Setup Grunt.js
keywords: javascript, grunt, grunt.js, websanova
description: I have never really gotten into using Grunt, but once I got it going I found out how great it is at automating so many tasks like linting, testing and minifying my files.
date: June 24 2013
permalink: /blog/javascript/how-to-setup-grunt
---

I have never really gotten into using Grunt, but once I got it going I found out how great it is at automating so many tasks like linting, testing and minifying my files. I actually came across it because I was trying to update one of my projects that had Grunt setup by someone else and so I tried to get it working locally. This led me to a lot of head scratching and confusion as apparently since version 0.4 of Grunt a few things had changed, however once I got everything working it was actually quite simple.

I was not able to find a simple concise guide on setting up Grunt as most of the information was quite scattered so I decided to write this little top to bottom guide. Grunt is actually a great tool and very easy to use. It will also save you lots of time fixing bugs and trying to manually do things like linting or minifying. I hope this guide will encourage you to setup Grunt for your project, so without further adieu.

## Install Node.js

Grunt requires you to install nodejs to install Grunt itself and it’s plugins.

* [Download Node.js](http://nodejs.org/download)

## Install Grunt

Now we can install `grunt` using the `nodejs` package manager.

~~~
$ npm install grunt --save-dev
~~~

To execute Grunt we’ll simply type `grunt` at the command line however if we’re using Windows we’ll need to type `grunt.cmd`. So for Windows also install the `grunt-cli`.

~~~
$ npm install -g grunt-cli
~~~

## package.json

Your project will require a file called `package.json` which will contain some information Grunt will use when running tasks. The key part here is the `dependencies` section which makes it easy for anyone else working on your project to install the appropriate modules required for the project. Here is a sample from my [js-url](https://github.com/websanova/js-url) project.

~~~
{
  "name": "js-url",
  "version": "1.8.0",
  "description": "A simple, lightweight url parser for JavaScript.
  "main": "js-url.js",
  "repository": {
    "type": "git",
    "url": "https://github.com/websanova/js-url"
  },
  "author": {
    "name" : "Websanova"
  },
  "homepage" : "http://www.websanova.com",
  "license": "MIT, GPL",
  "dependencies": {
    "grunt-contrib-qunit": "",
    "grunt-contrib-uglify": "",
    "grunt-contrib-jshint": ""
  }
}
~~~

## Installing Grunt Plugins

There may be some confusion about this as previously the modules were installed globally but as of version 0.4 of Grunt they are now installed on a per project basis. So with our dependencies set in the `package.json` file we can now easily install all our Grunt modules by simply running `npm install` from the project directory.

~~~
$ npm install
~~~

Note that you may get a warning here about `package.json` but don’t worry about that.

From here we can add or remove dependencies and I recommend looking at the [Grunt plugins](http://gruntjs.com/plugins) page for a full listing of plugins available along with instructions on how to use each one.

## Ignoring Modules

Since you should now be using Grunt 0.4 the modules will be installed in the project directory. If you’re using any version control system like Git then we will need to ignore the `node_modules` folder and the `npm-debug.txt` file so that these don’t get committed as part of your project.

~~~
node_modules/
npm-debug.txt
~~~

## Setting up a Gruntfile

The `gruntfile.js` is where you will setup how the plugins will work and which plugins to execute. When you run `grunt.cmd` it will automatically look for this file and run it accordingly. Note that in previous version this file may have simply been named `grunt.js` so a rename may be necessary. Below is a sample Grunt file that matches the `package.json` file above from my [js-url](https://github.com/websanova/js-url) project.

The key thing to note in the sample below is the `loadNpmTasks` which are your plugin names and should match your dependencies (if you plan to execute them). You will notice small configurations for each plugin. The best way to find out how to configure your plugins is to view the [Grunt plugins](http://gruntjs.com/plugins) page for detailed information on how to configure that plugin.

~~~
module.exports = function(grunt) {
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    qunit: {
      files: ['index.html']
    },
    jshint: {
      options: {
        curly: true,
        eqeqeq: true,
        immed: true,
        latedef: true,
        newcap: true,
        noarg: true,
        sub: true,
        undef: true,
        boss: true,
        eqnull: true,
        globals: {
          'window': true,
          'jQuery': true
        }
      },
      files: {
        src: ['./js-url.js']
      }
    },
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> - v<%= pkg.version %> - <%= grunt.template.today("yyyy-mm-dd") %> */'
      },
      my_target: {
        files: {
          './js-url.min.js': ['./js-url.js']
        }
      }
    }
  });

  grunt.loadNpmTasks('grunt-contrib-qunit');
  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-uglify');

  grunt.registerTask('default', [ 'qunit', 'jshint', 'uglify' ]);
};
~~~

## Executing Grunt

Once you have everything setup running Grunt is very simple and in the end only requires three basic steps.

* Clone a repo.
* Run `npm install` from project directory to install plugins.
* Run `grunt.cmd` to execute tasks

And voila, there you have it. If you have any further questions just leave me a comment below.