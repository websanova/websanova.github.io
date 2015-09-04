---
layout: master
title: Creating a New Node.js Project
keywords: nodejs, project, package, json, package json, npm, install, npm install, npm update, update
description: So you've decided to dive into Node.js but you're not quite sure how to start. This article covers the simple process of getting your feet wet with Node.js.
date: Mar 19 2014
img: /wp-content/uploads/2014/03/creating-a-new-nodejs-project.png
permalink: /blog/node-js/creating-a-new-node-js-project.html
---

As we saw in my previous tutorials on [Installing Node.js with Nginx Proxy](/blog/nginx/installing-node-js-with-nginx-proxy) and [Serving Dynamic Pages with Node.js](/blog/node-js/serving-pages-dynamically-with-node-js) it’s quite easy to get Node up and running in no time. However if we really want to take full advantage of Node.js we’ll want to familiarize ourselves with it’s package manager `npm` for short. With the package manager we can install any libraries available including the popular `express` framework by simply creating a dependency for it and typing `npm update`.

To start we will have to create a file called `package.json` in the root of our project. We can do this two ways. One is to just copy and paste a generic template into a `package.json` file. The second way is to navigate to the root of our project and type `npm init` which will ask you a few questions and create the file for you.

~~~
/path/to/project/npm init
~~~

Either way you go you will probably need to edit the file anyway. My preference is to just copy and paste as using the `npm init` command generates the file with additional dependencies and options that we don’t necessarily need. In the end at a minimum we want to start with something like this:

~~~
{
  "name": "myproject",
  "title": "My Project",
  "version": "0.0.0",
  "description": "My project is great.",
  "main": "",
  "author": {
    "name": "Rob",
    "email": "rob@websanova.com",
    "url": "http://websanova.com"
  },
  "homepage" : "http://myproject.com",
  "dependencies": {
    
  }
}
~~~

As you can see most of these fields are generic and can be filled out based on your project names and so forth.

With the dependencies it’s as easy as finding the package you want to install in the [node package manager directory](https://www.npmjs.org) and entering it’s name. The value to the right of the name indicates the version number following the [semver format](https://www.npmjs.org/doc/misc/semver.html) allowing you to specify a particular version of a package should you need to. Otherwise just leave it blank and it will pull the latest version available.

~~~
{
  ...
  "dependencies": {
    "express": ""
  }
}
~~~

The great thing here is that if we want to update our packages we can just type `npm update` and it will automatically update all our packages to the latest versions for us.

Note that we could also install a package directly by typing the packages name. The problem with this however, is that it won't be registered within our project so if we wanted do an update we would need to run the command on every package we would want to update.

~~~
npm install <package_name>
npm update <package_name>
~~~

There you have it, again the great thinng with `Node.js` is it’s simplicity. As you can see setting up a project is as easy as dropping in a single file, setting some dependencies and typing out a single command, `npm update`. This keeps things really simple and allows us to get a good idea of what is going on in a Node project by just looking at its `package.json` file.