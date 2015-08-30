---
layout: master
title: Writing Unit Tests in Node.js
keywords: nodejs, node, assert, tests, unit, strict, equal, strictequal, node.js, deepequal
description: An important part of Node.js will be writing unit tests for your apps and it’s modules. Like most of Node.js the basics of setting this up is a snap and you can begin writing unit tests in not time.
date: Mar 25 2014
permalink: /blog/node-js/writing-unit-tests-in-node-js.html
---

An important part of Node.js will be writing unit tests for your apps and it’s modules. Like most of Node.js the basics of setting this up is a snap and you can begin writing unit tests in not time. All we need to do is include the [assert library](http://nodejs.org/api/assert.html) that already pre-ships with Node.js. Then from thered we can go ahead and create a test file with a series of assertions to test our code.

As mentioned we’ll begin by including our `assert `package.

~~~
var assert = require('assert');
~~~

To simplify our testing and to give us some feedback on what is being tested we will create a basic function that will house our assert call.

~~~
function strictEqual(a, b) {
  console.log('Test: ' + a + ' === ' + b);
  assert.strictEqual.apply(null, arguments);
}
~~~

From here we can go ahead and create our assertions. We can create as many or as little as we like but the more the merrier.

~~~
strictEqual(1, '1');   // false
strictEqual('1', '1'); // true
strictEqual(1, 1);     // true
~~~

To run the tests simply navigate to the directory with your test file and execute it using node.

~~~
node test.js
~~~

If you have a failed assertion you will get an error alerting you that there is a problem with your code. We could of course make our tests much more complicated with many different types of assertions in multiple files. However this should be enough to get your going and for the most part will cover your testing needs.