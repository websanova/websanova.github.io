---
layout: master
title: Getting Started With GitHub Pages
keywords: github, pages, gh-pages, repository, jekyll
description: A short guide describing how to setup a simple blog on GitHub Pages.
date: Dec 08 2014
permalink: /blog/github/getting-started-with-github-pages
---

Getting a blog setup on GitHub Pages is actually quite simple...once you know how to do it. However I found the basics on this quite confusing and didn't find a concise A to B article to get something basic going. Not to mention throwing in Jekyll in there and figuring out how that ties in.

So without further delay here are a few simple steps to get you from no blog on GitHub Pages in no time. For starters let's not confuse things and forget about Jekyll for now.

## Creating GitHub Pages Repository

First thing we'll do is setup the repository. the [starter guide](https://pages.github.com/) is pretty good so we'll just do a quick recap here.

When you create an account in GitHub you will have a username. For instance mine is `websanova` which can be found at [https://github.com/websanova](https://github.com/websanova). In that account you will need to create a repository named `<username>.github.io`. So for instance my Github Pages repository is called [websanova.github.io](http://websanova.github.io).

If you do not name it exactly as your account name it will not work. Also __it may take up to 20 minutes for the site to activate__. Actually I think it took closer to about 40 minutes for me so hang tight.

Once setup we should be able to navigate to `http://<username>.github.io` and we can also view the full repository itself at `https://github.com/<username>/<username>.github.io`. For instance you can view my blog repository directly at [https://github.com/websanova/websanova.github.io](https://github.com/websanova/websanova.github.io).

Keep in mind that your blog is public and will always run under `http`, so don't keep anything personal in there.

## Setting up a Custom Domain

Let's take a look at setting up a custom domain for our blog. Now of course ther are a ton of ways to do this. But let's just look at the most basic.

First thing we'll need to do is to point the domain to GitHub's servers. So we'll need to edit our DNS records. This is a pretty common setup so just edit the following records.

~~~
  A        @     192.30.252.153
  A        @     192.30.252.154
CNAME     www    websanova.github.io.
~~~

Next we'll have to create a special file in our blog named `CNAME`. It must be all upercase and be in the root of your folder. This file should contain absolutely nothing else other than your domain name. Feel free to navigate to my [blogs repository](https://github.com/websanova/websanova.github.io) for inspection.

This may take a bit of time to update but should be relatively fast. Now if we go to our domain [http:://websanova.com](http://websanova.com) or [http://www.websanova.com](http://www.websanova.com) they should both point to the blog. Where for now you will still have a 404 Not Found page.

The last thing you may be thinking about is that you want your blog to either be strictly `www` or `non-www`. This is handled automatically for us by GitHub. You can specify which will be your default by the domain name you use in the `CNAME` file. So if you put `websanova.com` in the `CNAME` file then `www.websanova.com` will redirect to it and vice versa.

As for email you may want to look into using a service like [mailgun](http://mailgun.com).

## Publishing Pages Live

Now we come to actually putting some content live. Here we'll talk about Jekyll a bit. Basically it's a little script engine that will compile your pages to static html files from the files you create.

Now to do this we'll need to follow a bit of a special setup. So for starters we'll need to create a `gh-pages` branch in our repository. So literally just create the branch buy stay in `master` and do nothing else. It's just a holder branch for the static content.

Next we'll have to create a `_layouts` folder which is a default folder Jekyll uses to look for layouts. In there we can create a `default.html` file and put our basic layout inside.

~~~
<html>
<head>
    <title>{% raw %}{{ page.title }}{% endraw %}</title>
</head>
<body>
    <h1>{% raw %}{{ page.title }}{% endraw %}</h1>
    {% raw %}{{ content }}{% endraw %}
</body>
</html>
~~~

Next we'll create an `index` file in the root of our folder. This file can be either `html` or `md` based on whatever style you wanna follow. This file contains some special syntax at the top that we can use to specify some variables for the page

~~~
---
layout: default
title: Home
---

Hello World!
~~~

Again, you may inspect my [blog repository](https://github.com/websanova/websanova.github.io) for assistance as I follow a very simple setup.

Now we can just push these files live and we should see our index page. From here you can follow the same pattern for any page you create. To link to them just use the file name. __It's a good idea to use SEO friendly slugs for file names__ becasue of this.

Note that the project root is just like any other website root. So we can drop things like a favicon.ico or folders and files in there and they will get picked up normally.

## Adding a Custom 404 Page.

Adding a custom 404 page is quite simple and the [guide on GitHub](https://help.github.com/articles/custom-404-pages/) is pretty straight forward.

All you need to do is create a file in your project root called either `404.md` or `404.html`.

In the page settings just like on any other page you can set the `layout` and `title`. But you MUST also set an additional `permalink: /404.html` parameter. The page should look like the following:

~~~
---
layout: default
title: 404 Not Found
permalink: /404.html
---

The page you have requested is not found.
~~~


## What is Jekyll

I don't really know much about Jekyll other than a few basic tags. In fact I don't even use Jekyll locally to build this article or blog initially. But suffice it to say if you follow it's syntax it will convert your pages to static html pages.

For now GitHub does this for you when you push your content live. However itt comes as a Ruby Gem which means you can install it locally (with Ruby) and run the script on your pages locally to test them before pushing live to GitHub. A good step to get to eventually to avoid a bunch of fixer commits.