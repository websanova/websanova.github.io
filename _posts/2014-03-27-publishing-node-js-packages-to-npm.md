---
layout: master
title: Publishing Node.js Packages to NPM
keywords: nodejs, node, packages, package, npm, publish, javascript
description: So how exactly do you make your JavaScript extensions, libraries and plugins work on Node.js. It's quite simple and we cover it in this article.
date: Mar 27 2014
permalink: /blog/node-js/publishing-node-js-packages-to-npm
---

So you’ve created a new shiny Node.js package and now you want to share it with the world. So where do you start? Well it’s actually quite simple using Node and all you really need is a valid `package.json` file. You can check out my [Creating a New Node.js Project](/blog/node-js/creating-a-new-node-js-project) post to help get things started. From there a single command will push your package to the NPM directory and make it available for anyone else to use within a matter of seconds.

To start you’ll want to make sure you have your user info set before publishing and it’s probably a good idea to match the account info you have at [npmjs.org](http://npmjs.org/).

~~~
npm set init.author.name "websanova"
npm set init.author.email "rob@websanova.com"
npm set init.author.url "http://websanova.com"
npm adduser
~~~

Then you’ll need to create your `package.json` file. It should be located in the root directory and should look something like this:

~~~
{
  "author": {
    "name": "Websanova",
    "url": "http://websanova.com",
    "email": "rob@websanova.com"
  },
  "dependencies": {},
  "description": "A simple url parsing library.",
  "licenses": "MIT, GPL",
  "devDependencies": {},
  "keywords": ["util", "url", "parse"],
  "main": "wurl.js",
  "name": "wurl",
  "repository": {
    "type": "git",
    "url": "https://github.com/websanova/node-url"
  },
  "version": "1.1.0"
}
~~~

Most of the information is pretty standard. The most important fields to pay attention to are the `main` and `name` fields.

The `name` field will be the name the package will be published under. The biggest issue you’ll face here is conflicts with the name since they don’t have namespacing with user names like in GitHub or other services. I like to just use a `w` in front of packages that contain a conflict to denote it’s a Websanova package.

The `main` will be the file that is run when including the package in your Node.js code. You will notice the full path name is not required.

~~~
var wurl = require('wurl');

console.log(wurl('?name', someurl));
~~~

Finally, all you need to do is run the following command in the root directory. It will automatically look for the `package.json` file and attempt to publish the package. If any errors occur it should give you a nice warning message telling you why.

~~~
npm publish ./
~~~

From here you can keep re-publishing using the same command. You will just need to make sure you increment the version number each time you publish otherwise it will throw an error.

You can also create a `README.md` file that will automatically be picked up and published on your packages home page. It follows standard markdown syntax and can be used on both GitHub and NPM.

Finally, if you’re not felling lazy you should also probably create some simple tests for the package. You can check out my [Writing Unit Tests in Node.js](/blog/node-js/writing-unit-tests-in-node-js) post on how to write test. You can also take a look at my simple [node-url package on GitHub](http://github.com/websanova/node-url) as a simple sample to follow.