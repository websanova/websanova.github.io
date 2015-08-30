---
layout: master
title: How to Design a REST API and Why You Should
keywords: design, rest, api
description: Detailing the process of designing a REST API and why you should build one in the first place.
date: Jul 17 2012
---

### Intro

I have spent the last year or so learning and implementing backbone.js, REST and API development (among other things).  They have really grown on me and have greatly simplified my life not only as a developer but as a team lead.  It's like when you went from no cell phone to a crappy little flip phone and then from that crappy little flip phone to a smart phone.  Although you might trade your Android phone in for an iPhone and vice versa there is just no going back.  It's the same with backbone.js and REST.

I decided to write this article because I would never build another web service/app without first creating an API right off the bat.  Even if the API is not public and 100% internal, it serves as a very powerful guiding piece for your entire application development and management.  The truth is, it's not a very difficult concept and for the most part it's just a reshuffling of a lot of ideas you already know and understand about web development.  My goal is to give anyone interested a solid starting point from which to move forward with and start implementing immediately.

### The Why

Here are just a few reasons why I think the REST design model is great.

- Works Out of the Box</b> - There is nothing you have to setup, it's just a design model, a pattern to follow that is built right into HTTP itself.
- Standardize Development</b> - 95% of your code will follow a POST/GET/PUT/DELETE model.  You will not fully realize the benefits of this until you try it yourself.
- Standard API</b> - Creating an API on standards that already exist makes it much easier for application developers to use your API.  The easier it is to use the more apps they will build.
- Great For New Employees</b> - Since everyone follows the same model it is MUCH easier to jump right in and start developing.  Developers already familiar with the concept will already understand your code.  For those that aren't familiar with REST, relating that CRUD = POST/GET/PUT/DELETE is not difficult to understand.
- Planning</b> - It makes it much easier to plan out new components.  Most places I have worked will usually give you high level specs at the interface level  Then developers start thinking about how to abstract that data into a database.  The part that's usually missing is how you go from interface to data.  With REST it's very easy to plan all this out and find a lot of the missing details.
- Better Estimations</b> - Directly related to the point above, because we can plan the implementation details better with a series of REST calls we will need to make, we can better outline the tasks required.  This helps keep things in smaller more deliverable pieces that can be integrated to the main code base easier and more frequently.
- Adding Code</b> - Having an API means your web site is pretty much just an app built off that API, this makes it super easy to build other apps.  Say you start with a website, well with an API, you can build out a mobile app much faster and easier, you're just building an interface on top of a standard API.
- Growth</b> - Creating a truly stateless API makes it much easier to scale and grow your infrastructure allowing you to easily add and remove web servers on the fly.
- Morale</b> - You will spend much less time rehashing existing code, which means spending more time writing new code.  You can also confidently delegate more responsibility to your team.  I guess I can't speak for everyone but both of these make me feel more productive and like I achieved something at the end of the day.
- Difficulty</b> - It's really not that hard, the amount of benefits you gain from it far outweighs the little amount of time you will spend learning the basic concepts of REST.

### What is REST

To start off I think it's important to be aware of what exactly REST means. REST stands for REpresentational State Transfer which just means that each request you make to a server contains all the information (state) it needs for your server to fulfill the request.  Most web applications and many popular APIs are actually hybrids of REST and RPC.  Here is a nice definition for both from "RESTful Web Services" by Leonard Richardson and Sam Ruby. 

> A RESTful, resource-oriented service exposes a URI for every piece of data the client might want to operate on.  A REST-RPC Hybrid exposes a URI for every operation the client might perform: one URI to fetch a piece of data, a different URI to delete that same data

A hybrid REST-RPC will always use GET and have different URIs for the same piece of data. 

~~~
GET /items/:id?method=delete
GET /items:/id?method=update
~~~

A RESTful service will have the same URI for a piece of data regardless of the operation.

~~~
GET /items/:id
PUT /items/:id
~~~

This is the core of what makes a service truly RESTful and we will be following this as our guiding principle.

### What is SOAP

I thought it would be good to just quickly highlight what SOAP is as it is probably the most directly popular alternative to REST.  SOAP is basically an RPC style approach except that it only exposes one URI  for your entire API.  All the remaining logic for methods, parameters and data is contained in an XML file that is sent to the URI.  The server then parses this XML and decides what to do with it.

Personally I don't see the point in wrapping an extra layer around what is already provided by HTTP itself.  In my opinion REST is even better than SOAP and requires no additional logic, it works out of the box and is how the internet is supposed to work.  SOAP just adds additional logic on both the client and server.  Once to assemble the message for transport and then once to take it apart and figure out what to do with it.

Also providing a RESTful interface makes it much easier for developers to plugin to your API off the bat.  A simple CURL call will already get you going.  A good API should be as easy to use as possible and wrapping extra logic around, in my humble opinion, just doesn't make any sense.

### What's All This Stateless Gobbledygook

One big area of confusion around REST is the concept of statelessness.  What this means is that each request you make to your server is completely independent and isolated.  It does not require any previous requests for any future request to happen.  Everything that is required to make the request is sent with the request itself, hence the term REpresentational State Transfer.

Simply put, this means you should not use any server side sessions to keep track of state.  For anyone who hasn't worked in a large environment and is new to REST this seems plain crazy.  If we're not using sessions that would mean we would have to authenticate each and every request made to the server.  This seems like a lot of waste as it requires an extra hit to the database on every single request.  The reason for this is to allow your infrastructure to grow making it much easier to add new servers to your cluster without worrying about session management across multiple servers.

Basically it's a trade off.  If you have one or two servers then the cost of session management will be less than that of authenticating each request to your servers.  However as you add more servers to your environment the cost of session management increases.  In theory there should be a sweet spot where it's actually more efficient to authenticate each request than to manage sessions across all your servers.

From a real world point of view.  If you have a small service with only one or two servers, then using sessions makes sense especially if your API is internal and only acts as a means of organizing your code.  If you grow and find yourself needing to be able to add servers on the fly with zero overhead then making your service completely stateless will make more sense.

### Overview

When developing an API there are basically three main steps:

- Plan out your URIs
- Setup return values and response codes for your URIs
- Implement a framework for your API.

I'm not going to cover too many the implementation details as it's not as important and can be done many ways depending on what framework you're using to begin with.  What's important is designing our external interface properly.  We need to create URIS and return data in a consistent and intuitive manner

I'm going to assume you are using some sort of MVC or similar type of framework, so I will be referring to controllers a few times throughout the article.  For those that aren't familiar with MVC you should probably read up on that first.

### CRUD = POST, GET, PUT, DELETE

The first thing you need to know is the how the request methods map to the basic CRUD operations. They are as follows:

~~~
Create	=> POST
Read 	=> GET
Update	=> PUT
Delete	=> DELETE
~~~

Depending on your framework the URIs will typically map to a controller and a method name specified by the type of request method.  So a POST call to /users will look for a method `post()` in the users controller and so on.

For those new to this it may sound a little extreme to stuff all your calls into just four methods, but hopefully I'll show you it's not as crazy as it sounds.  If it's your first time building an API, you will definitely be tempted to move away from this pattern and create "exceptions".  I would advise you not to as there is usually a way to adhere to the four methods above.

Typically the standard pieces are the POST and DELETE calls since they usually only ever represent one action for one resource.  Where it usually gets sticky is with the GET and PUT calls.  For put calls we typically see a series of methods such as:

~~~
update_order()
update_title()
etc...
~~~

You're actually updating the same resource so we can stick all these into one method and just pass the data we want to update instead so:

~~~
function put($id)
{
	$order = $_PUT[‘order'];
}
~~~

Again depending on your framework the implementation will be different, but the idea is that you access the same resource URI but just pass it the data you want to update.

Then for GET calls we might see something like

~~~
get_reviews_full()
get_reviews_simple()
get_review()
~~~

Same idea goes here, much simpler to have one function with a "filter" parameter that will handle the different types of data you want to send back.

~~~
get($id, $filter)
{
	if($filter == ‘full')  / get full
	elseif($filter == ‘simple') //get simple
	etc...
}
~~~

### PUT vs POST

There is one little detail I wanted to point out between PUT and POST to remove any confusion you may get if you see a PUT request being sent as a POST request. From "RESTful Web Services":

> The client uses PUT when it's in charge of deciding which URI the new resource should have.  The client uses POST when the server is in charge of deciding which URI the new resource should have.

The typical example here is user registration.  Perhaps you want to have a direct URI to users profiles that is easy to read, so you may have a request that looks like

~~~
PUT /users/george
~~~

Which would then be accessed using http:/www.domain.com/users/george

In this case the user is deciding what the URI is and not the server.  If it were the server we would see something like:

~~~
POST /users
~~~

with a payload of &username=george and a URL of:

~~~
http:/www.domain.com/users/:id
~~~

This is not really a REST issue and more of a preference issue on implementation and how you want to design your API.  I mention it only to keep you from getting confused should you come across it.

### Basic Patterns

Setting up some basic URIs may seem very simple at first, but it really depends on your situation and what you're trying to achieve.  Assuming that you are using the currently authenticated user for each request we could do something like the following:

~~~
POST /users
GET/PUT/DELETE /users/:id

POST /projects
GET/PUT/DELETE /projects/:id
~~~

We don't need to include `/users/:id` under the `/projects` URI, because we already know the user.  It is very rare that you will need to make a request as one user on behalf of another user.  In fact I can't even think of feasible real world situation where you would want to.  But for the sake of argument, if you did then you will most likely need to include the extra `/users/:id` part in the URI.

~~~
POST /users/:id/projects
GET/PUT/DELETE /users/:id/projects/:id
~~~

### Clean URIs

Let's expand our URIs now and take a look at adding tasks to a project as an example.

~~~
POST /projects/:id/tasks
GET/PUT/DELETE /projects/:id/tasks/:id
~~~

At first glance it all seems to make simple sense.  If I want to create a task or get all the tasks in a project I will need a project id, so our POST and GET calls happily pass one.

How about the PUT and DELETE calls though, if I just want to access the task resource I don't really need the project id.  We probably just need to make sure the user has access to perform that operation and we can do that using the currently authenticated user.  Why not just make those calls like so:

~~~
PUT /tasks/:id
DELETE /tasks/:id
~~~

In some situations we probably could, but in many cases we will still need that extra project id parameter.  Maybe we want to make sure the project is still active before we perform any operations on it.  In this case it saves us an extra look up to the tasks table in order to retrieve the project id.  I find I end up using that extra id more often than not, so I always toss it in, even if it may be a little redundant.  Plus it just makes for a much cleaner looking and more consistent interface.  If they need the value they know it will be there.

### Scoping

When you begin setting up URIs for your API you may come across some situations where you may be uncertain as to whether to put a parameter into the URI or to set it as a query variable.  From the server's point of view, it doesn't really care how you pass your information as long as it gets it.

Say we want to delete a task from a project, we could do it in two ways:

~~~
DELETE /projects/:id/tasks
DELETE /tasks/:id?project_id=x
~~~

Both are restful as they both represent the task as a single addressable resource.  In many situations this will work out perfectly fine.  However, the reasons to keep the scoping information in the URI is to make it more human readable and to represent your resources in a hierarchical way.

Think about it like a directory structure with sub folders and files.  A task will only ever belong to one project.  The question can be where do we draw the line?  Say I want to run a search on my tasks, should we allow something like the following?

~~~
GET /projects/:id/tasks/api
~~~

In this case it makes more sense to put it as a query variable like so:

~~~
GET /projects/:Id/tasks?q=api
~~~

The "api" string doesn't represent any particular part of our system.  Its variable and has an infinite amount of possibilities. From "Restful Web Services":

> Path variables look like you're traversing a hierarchy, and query variables look like you're passing arguments into an algorithm

There is one more situation however that sits on the fence between the two and it's called a filter. Say we want to get all tasks that are marked as bugs:

~~~
GET /projects/:id/tasks/bug
~~~

Should this be in in the URI or a query variable.  In this case I would keep it in the URI as it's a consistent representation of group of resources.  Also you could think of it as a bug folder inside of projects so that it fits in with our hierarchical design.

### Mapping URIs to Controllers

I hope you made it this far and didn't get too excited about starting on your API yet.  This is a very simple but important piece that will help you keep your backend organized.

Now there are many, many ways to implement your API, but I find it's best to have one controller for each resource, for example users, projects and tasks.  Don't worry about mapping your URIs directly to your controller layout.

The best way to do it is to reroute your requests to the appropriate controller so that:

~~~
GET /projects/:id/tasks
~~~

would actually be a call like /tasks/:project_id and will call a function in a task controller that might look something like this:

~~~
get($project_id)
{
    //code
}
~~~

and a request to update a task:

~~~
PUT /projects/:id/tasks/:id => /tasks/:project_id/:task_id
~~~

and map to a function inside of the task controller like so:

~~~
put($project_id, $task_id)
{
    //code
}
~~~

### Keep URls Consistent

Sometimes it's tempting to just shorten our URIs like in the case I described above with PUT and DELETE for tasks.  We may think the project id is redundant so we'll just leave it out but maintain the url hierarchy, so you'll setup a url like so without the project id:

~~~
PUT /projects/tasks/:id
~~~

I would not recommend this as it can make things very confusing if you don't follow a consistent pattern, also how will you tell if tasks is an id or just tasks.  It may depend on your framework but this will probably throw your redirects out of whack big time.  The key is consistency which will become increasingly important as your API grows.

### Payload Data

Any other data that is not in your url should be sent in the body of your request.  You should probably only ever send any data with your POST and PUT requests as they contain additional data for your request that should not be in the URI.  Think about creating a project with a title parameter.

Your GET and DELETE request should contain all the information they need in the URI, typically just some id's for the specific resource you want to interact with.  Remember we want to have a separate URI for each resource so we want to keep this clean.

### n-naming"><h2>Join Naming

Now let's take a look at the following two URIs:

~~~
GET /users/:id/projects
GET /projects/:id/users
~~~

The first one says get me all of this user's projects, so we take a user id and get a list of projects.  The second one says get me this projects users, so we take a project id and get a list of users associated with it.

Now let's think about how this will probably be setup on your backend.  You will probably have a table for users and a table for projects.  Then you will have a join table with a user id and project id to keep track of what users are a part of which project, representing a many to many relationship.

Now what happens if we want to add or remove a user from a project, which url should this go under?  We are not adding a user nor a project, so it doesn't make sense to really stuff things into either of those URIs.  What we have here is a new resource, let's call it "members" that will represent users belonging to a project and the URIs should look like this:

~~~
POST /projects/:id/members
GET/PUT/DELETE /projects/:id/members/:id
~~~

Here members will represent that join table and the operations that will be performed on it.  A simple guiding rule you should use is that if you are interacting with data in a specific table then you should probably have a URI to represent that resource in the table.  In join cases, try to find a connector word like members, contributors, readers, commenters, friends, etc, and use that to distinguish the operation.

### One to One Mappings

As you develop and grow your API you will begin to start having a lot of URIs.  In some cases I find it a good technique to group things together.  It can make things much easier conceptually and keeps your API from growing unnecessarily.  Take an example like users and a user's preferences.

Based on what we've seen so far we will probably decide to build the urls in the following way:

~~~
GET /users/:id
GET /users/:id/preferences
~~~

However that doesn't make much sense, we will never really add a new preference record and we will probably be using the user id to access preferences anyway.  A user and their preferences are directly connected one to one even though they may be in separate tables on your backend.  In these cases I find it best to combine the two and if you are updating a preference to always do a PUT call on the `/users/:id` URI.

On the backend we can build out a dynamic validator, so basically we check if a parameter is set and if so it gets validated.  From there we have some arrays that define what parameters belong to what tables and we can run a loop to split up the values into separate arrays and then update the appropriate tables.

### More URL Patterns

So far we have covered some pretty basic more common examples.  But I wanted to briefly cover some other patterns that you may run into.  Remember that there is nothing wrong with your url's getting too long and typically we will reroute all those to params in one function anyway.

Users favorite songs:

~~~
GET /users/:id/songs/favorites
~~~

In this case rather than an id we are passing a filter here, "favorites". 

List of users playlists that have been voted up:

~~~
GET /users/:id/playlists/:id/voteup
~~~

A list of users listening to a specific album:

~~~
GET /albums/:id/listeners
~~~

A list of users who have this album as a wishlist:

~~~
GET /albums/:id/listeners/wishlist
~~~

It's rare that you will get into situations with three keywords and even more rare that you will find one with four.  The best way is just to write it out in a way that makes the most sense and if you're unsure just ask a few people.

### Returning Data

How you return your data is up to you, but typically it will be either json or xml.  With many popular APIs you can usually specify the format you like in the url, so you will see things like:

~~~
GET /users/:Id/.json
GET /users/:id/.xml
~~~

It's usually pretty simple to convert your data to either format so it's a good idea to support both xml and json out of the box.  Also some more advanced APIs may have a language setting which will be represented as such:

~~~
GET /projects/:id/.en
GET /projects/:id/.sp
~~~

You could also combine these like so:

~~~
GET /projects/:id/.en.json
~~~

### Response Codes

Another important thing to keep in mind is to send proper response codes back to the client.  Do not just send back a 200 OK with every response and then some kind of "status=failed" or "status=success" parameter.  Although this may seem trivial at first, many frameworks and libraries such as jQuery have the use of response codes baked in, allowing you to really take advantage and leverage the libraries much more fully.  Also by following the standard it makes it much easier for other developers to plugin to your API and use libraries or SDK's they have already developed.

You can find a full list on [wikipedia HTTP status codes page](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes), however here is a list of the most common ones you'll use:

~~~
200 OK - General status for a successful request.
201 Created - Used on POST request when creating a new resource.
400 Bad Request - Typically if any validation for your parameters fails.
401 Unauthorized - Authentication is required to make this request.
403 Forbidden - Unable to access regardless of authentication or not.
404 Not Found - It someone access a resource or URI that doesn't exist.
409 Conflict - For POST and PUT requests if the resource already exists.
500 Internal Server Error - Any general error on the system.
~~~

### Versioning

Another piece to think about will be your API versions.  I find it a good idea to separate these into folders so that you can make copies when you plan on working on a new version.  This will give you backwards compatibility.  Keep in mind that if you release an api version to the public you should not be making too many modifications to existing APIs as this can affect how current apps are using it.

Depending on the framework you may need to route your requests based on the version, but for the most part, keeping your versions separate and organized should be enough.  For instance in CodeIgniter I have folders in my controller like so:

~~~
/api_v1
/api_v2
etc...
~~~

My URIs will be in the form, `/api/v1` so I will do a simple reroute to direct the requests to the appropriate folder.

### Authentication

I won't get into authentication too much, because that is a large topic on its own based on the type of authentication level you need.  There are many methods to authenticate a user based on what you're trying to achieve including basic, digest and oauth.  I'm currently diving into these topics myself and will cover authentication in a separate post in the future.

### Conclusion

I've attempted to only give you the core of what a RESTful web service really is.  But there is much more to it then what I've briefly covered here.

I would highly recommend reading "RESTful Web Services" by Leonard Richardson and Sam Ruby as they delve much deeper into many topics about REST including authentication and implementation details in various languages.

Also note that we have only covered 4 of the 9 request methods available in the HTTP protocol which include HEAD, GET, POST, PUT, DELETE, TRACE, OPTIONS, CONNECT and PATCH.

REST connects with many different concepts so my goal was to give you the foundation with the hopes that after reading this article you can immediately begin working on your own API.

I plan to eventually provide two supplementary pieces to this article on the following topics so stay tuned.

- Implementing authentication in REST, with focus on oauth.
- Implementing REST in CodeIgniter.

### Credit

I like to give credit where credit is due.  Special thanks go out to Leonard Richardson & Sam Ruby for their book "RESTful Web Services".  It really helped solidify much of my understanding about REST and API development.  It's a great book, go buy it!
