---
layout: master
title: 10 Reasons Why Laravel is Better Than CodeIgniter
keywords: laravel,codeigniter,framework,facade,eloquent,orm,blade,routing,rest
description: Yep another post on why you should switch to Laravel from CodeIgniter. I’ve been using CodeIgniter for about three years now and still continue to do so on some projects. However since I decided to finally give Laravel a whirl, it’s been no looking back.
date: Feb 11 2014
permalink: /blog/laravel/10-reasons-why-laravel-is-better-than-codeigniter.html
---

 
10 Reasons Why Laravel is Better Than CodeIgniter
Feb 11, 2014 by Rob | Leave reply
10 Reasons Why Laravel is Better Than CodeIgniter
Yep another post on why you should switch to Laravel from CodeIgniter. I’ve been using CodeIgniter for about three years now and still continue to do so on some projects. However since I decided to finally give Laravel a whirl, it’s been no looking back. I was quite impressed, Laravel is the framework PHP needed and it has been a breathe of fresh air to work with. Any new project will definitely be created using Laravel as it just ships with so much more out of the box than CodeIgniter does.

The key thing to note for those sitting on the fence is that the learning curve coming from CodeIgniter is very small and it only really takes a day or two to get up and going with Laravel. From there of course you will need to invest some time learning a few things here and there. But the time invested to learn a few new things in Laravel is well worth it in the long term as it will save you loads of time down the road.

This guide is not meant to be a tutorial on Laravel but only serves to give a feel for how some of the Laravel code looks like and works along with what the framework offers. I’ve seen a lot of posts praising Laravel over CodeIgniter but haven’t really seen any on why exactly it’s better so here are ten reasons (amongst many) that should help one get started.

## 1. Eloquent ORM

The way you design your models are based on relationships. This will really change the way you think about and design your apps. In CodeIgniter I typically set up my models in REST format basically having `post`, `get`, `put`, `delete` functions. With Eloquent everything is done for you. Just define relationships and never write anymore queries.

First we would define our relationship like so:

~~~
class Post extends Eloquent {
    protected $table = 'posts';

    public function comments() {
        return $this->hasMany('Comment');
    }
}
~~~

Then we can make calls like so:

~~~
Post::find($id)->comments()->get();
~~~

There is also a fantastic little security feature in Laravel that allows us to make fields in our tables guarded to avoid any potential missteps by developers. By default Eloquent won’t allow us to do a batch `post` or `update` of our models. We would have to do something manually like so:

~~~
$user = new User();
$user->first_name = Input::get('first_name');
// etc
$user->save();
~~~

However if we want we can just set the `$fillable` variable in our model and this parsing will be automatically done for us.

~~~
class User extends Eloquent implements UserInterface, RemindableInterface {

    protected $fillable = array('first_name', 'last_name', 'email');
}
~~~

We can than just run a batch input call like so:

~~~
User::find($id)->update(Input::all());
~~~

How awesome is that. Clean, secure and elegant. The thing I also really like here in Laravel over CodeIgniter is that when you create a new object it returns that obejct rather than just the `id` like CodeIgniter does.

This is just barely scratching the surface with what’s available with Eloquent. But the clean format makes it a lot of fun and much easier to use. Your can of course do `joins` and more sophisticated queries but it all follows the same simple syntactically sugary style.

## 2. Routing

Routing in CodeIgniter is very primitive compared to that of Laravel. In CodeIgniter you get a routing file where you can setup some redirection and overwrites using `(:any)` and `(:num)`. This works great most of the time but Laravel supercharges this. It’s like routing on steroids compared to CodeIgniter. At the most basic level we will see a route like this which just redirects to a controller.

~~~
Route::controller('posts', 'PostController');
~~~

However we can also add functions to it allowing us to handle requests right in the route! We could pretty much write our entire up in the routes file alone if we really wanted to.

~~~
Route::get('logout', function() {
    if (Auth::check()) {
        Auth::logout();
        return Redirect::to(Config::get('app.url.www') . '/');
    }
    else {
        return Response::view(
            'layouts.master',
            array('view' => 'errors.404')
        , 404);
    }
});
~~~

I also like that we can filter out URLs using regular expression patterns and automatically route them if they don’t match. This is great as a lot of the time we can just redirect to a 404 page if a value is not a number which is the case in good portion of our requests especially if we’re building out an API.

~~~
Route::get('dictionary/{letter?}', 'DictionaryController@getIndex')
     ->where('letter', '[A-Za-z]');
~~~

On top of that we can also create filters separately and attach them to any route we want. This is great for things like an admin or any other parts of your application requiring some sort of authentication.

~~~
Route::filter('auth', function() {
    if (Auth::guest()) {
        return Redirect::to('login')
                       ->with('error', 'You must be logged in.');
    }
});

Route::when('profile', 'auth');
Route::when('profile/*', 'auth');
~~~

Again I feel like I’m barely scratching the surface here. The thing I really like about it all is that you can put all of this control flow logic for your app into two files, `filters` and `routes` which really gives you an Eagle’s eye view of your entire app.

## 3. REST

If you’ve built any kind of REST application using CodeIgniter you’ve undoubtedly used or at least come across [Phil Sturgeon’s REST Implementation for CodeIgniter](http://philsturgeon.co.uk/blog/2009/06/REST-implementation-for-CodeIgniter). It’s quite easy to pop in and use immediately. However having something baked right in makes life just a little bit easier.

Laravel ships with a REST implementation and we can create a generic route that automatically maps to resources in the controller. For instance in our `routes.php` file we can just create a route like so:

~~~
Route::resource('posts', 'PostsController');
~~~

This than automatically maps to seven methods in our controller.

~~~
class PostsController extends \BaseController {

    public function index() {
        // Display a listing of the resource.
    }

    public function create() {
        // Show the form for creating a new resource.
    }

    public function store() {
        // (POST) Store a newly created resource in storage.
    }

    public function show($id) {
        // (GET) Display the specified resource.
    }

    public function edit($id) {
        // Show the form for editing the specified resource.
    }

    public function update($id) {
        // (PUT) Update the specified resource in storage.
    }

    public function destroy($id) {
        // (DELETE) Remove the specified resource from storage.
    }
}
~~~

We can of course just delete the `create` and `edit` functions if we don’t want the interface components. We can also tuck this away into a `/controllers/api` folder if we want to keep our API controllers separate. Then just route like so:

~~~
Route::resource('api/posts}', 'api\PostsController');
~~~

## 4. Artisan

Artisan is a small PHP script in Laravel available from the command line that you can use to get some information about your app as well as auto generate and optimize code. A majority of the time you will be using the `migrate` command to update your database schema but it also has a bunch of other useful commands that you can see by running:

~~~
php artisan list
~~~

You can also [develop your own artisan commands](http://laravel.com/docs/commands) allowing you to extend it’s features. For instance I have a custom command that backs up my database that I run before running any migration commands.

There simply is nothing like this in CodeIgniter and although it’s quite easy to just copy and paste things like a controller having this feature, especially for migration is something that is hard to live without once you get used to it.

## 5. Migrations

With CodeIgniter there is no easy way to keep your schema synced across environments or developers. The typical way is to create a schema folder and just add any schema changes there into a file with a timestamp as it’s file name. This works for the most part but it becomes messy and is a bit of a hassle. Not to mention if you want to be able to rollback any changes it just increases the complexity.

If you start having a lot of schema changes or need to rebuild your schema then you either have to get a dump from someone or you need to run all the schema files one after another.

This is a nice little headache removed by Laravel. The `migrate` command allows you to easily keep your schemas in sync by running a few simple commands. The three commands you will be using most often will be:

~~~
php artisan migrate:make create_posts_table
php artisan migrate
php artisan migrate:rollback
~~~

The first command creates a new schema file that you can enter your changes into. The second command updates your schema with all the new schemas that have not yet been run. The third command just rolls back the last migration schema file that was run.

The nice thing is that when you generate a migration file it comes with two functions for updating and rolling back your change respectively called `up` and `down`.

~~~
class CreatePostsTable extends Migration {

    public function up() {
        Schema::create('posts', function($table) {
            $table->increments('id');
            $table->string('content', 1000);
            $table->tinyInteger('status')->unsigned()->index();
            $table->timestamp('created_at')->index();
            $table->timestamp('updated_at')->index();
        });
    }

    public function down() {
        Schema::dropIfExists('posts');
    }
}
~~~

There is also seeding functionality that you can optionally run to seed your tables with data. It’s setup quite similarly and can be run just as easy using the `–seed` option or running the following command:

~~~
php artisan db:seed
~~~

## 6. Composer

Composer is a sweet way to add and maintain packages in your app. You simply add some lines in your config specifying the package and some aliases, hit composer update and it will auto install for you. Also hitting update in the future will automatically pull the latest version for you. Of course you also have the option to keep a certain version of the package if you like.

If we open up our `composer.json` file we will see a `require` area where we can add packages. Here is an example with a user agent package I like to use for easily getting some information about the users browser.

~~~
"require": {
    "laravel/framework": "4.0.*",
    "mews/useragent": "dev-master"
},

We then add the following aliases into the the `config/app.php` file:

~~~
'providers' => array(
    'Mews\Useragent\UseragentServiceProvider',
    // etc...
),

'aliases' => array(
    'Agent' => 'Mews\Useragent\Facades\Useragent',
    // etc...
),
~~~

The ability to so easily find new packages and keep them up to date including the framework itself is a huge burden to be freed from. Not to mention that the packages will be far more stable as they are likely to already be used by many other developers.

The packages include everything from oAuth to Twitter and Facebook SDKs. There are a few sources for getting packages but it’s probably best to use the [official Laravel repository](http://registry.autopergamene.eu/).

## 7. Templating

I’ve never been a big fan of any templating systems. I always thought it added unnecessary overhead and who wants to learn a whole new syntax and way of doing things when PHP already works just fine. But truth is I really enjoy using Blade. It definitely has some nice shortcuts like the include function with it’s dot notation for getting views inside of folders. For instance if we wanted to get the `views/posts/index.blade.php`:

~~~
@include('posts.index')
~~~

You can echo any variable by simply using the `{{ }}` notation. Note that it’s simply executing php code with an `echo` in front of it.

~~~
{{ 'some' . $var . 'here' }}
~~~

I also like that it just keeps the code clean and makes it much easier to look it. It’s much better than using `` tags all over the place.

~~~
@if ($var === true)
  Yes it's true: {{ $var }}
@endif
~~~

This is just basic stuff but there are for more advanced things you can do like defining a `@section` in a master template and then overwriting it in sub templates. This gives you some interesting flexibility to overwrite components on a page.

CodeIgniter doesn’t provide any templating out of the box. It wouldn’t be too difficult to install something like smarty templating or any others that might be out there but again it’s just an extra step. Plus without Composer that extra step turns into more steps down the road for maintenance.

## 8. Authentication

I like that Laravel comes with all the functionality you need for registering and logging in your users. This is great and gets you started building a new app really fast. There are even some popular packages you can install that have the entire authentication and activation processes ready to go.

We can generate a hash for new users by using the hash class:

~~~
Hash::make(Input::get('password'));
~~~

We can then authenticate using the handy `attempt` method below where `email` can be either an email or `username`.

~~~
Auth::attempt(array(
	'email' => Input::get('email'),
	'password' => Input::get('password')
));
~~~

If the `attempt` method returns true and the user is logged in a session with the user object is automatically created for us which we can then access using:

~~~
Auth::user();
Auth::user()->id;
// etc...
~~~

We can also add additional items to this object if we like:

~~~
Auth::user()->admin = true;
~~~

It doesn’t get much easier than that, the code is secure and easy to use. The handy methods make dealing with user creation and authentication a breeze allowing us to quickly focus on building our app. With CodeIgniter we don’t have anything like this and again requires us to find a package that again we can’t maintain through any kind of packaging mechanism like Composer.

## 9. Facades

Without getting too much into it the idea is that all your objects are accessible through what appear to be static method calls using the facade design pattern. This is great and is what makes things like `User::find(1);` so clean and elegant to use. Everything in Laravel is handled this way but don’t be fooled by it, these are not static method calls. All the instantiation details are handled behind the scenes.

For instance if we were to create an object and then call a couple methods on it:

~~~
SomeObj::set('test', 3);
SomeObj::get('test'); // Should be 3.
~~

In the above example we would be referring to the same object and it all just happens magically for us. This is super clean and makes code much easier to work with. In CodeIgniter we would have to either set a library to `autoload` in the config or always include them in our controllers first. Below is an example with both CodeIgniter and Laravel.

~~~
// CodeIgniter
$this->load->model('users_model');
$this->users_model->get(1);

// Laravel
User::find(1);
~~~

In our implementation we used little wrapper methods in helpers to avoid this. But it’s just more code and more room for bugs and it doesn’t come out of the box. This is frustrating if you’re starting a new project and none of these little additions you’ve made our there anymore.

Facades are a bit of a more complicated subject if you want to start building our your own libraries but a good place to start is the [Tuts+ Tutorial by Jefferey Way](https://tutsplus.com/lesson/when-they-say-laravel-shouldnt-use-static-methods/) that gives a nice overview of it.

## 10. Mail

This is another thing that you will probably deal with in which I like the way Laravel implemented. First off the configuration for mail is in the `config/mail.php` file. This is not a big deal as we could simply add something like this in CodeIgniter. But then I also like the way we can easily attach templates to the email sent.

~~~
Mail::send('emails.activate', array('user' => $user), function($message) {
    $message->subject('Account Activation');
    $message->to($user->email);
});
~~~

In the above example we have the template we want to use, some data to pass into the template and a function that lets us modify some of the mail parameters which in this case is the subject and email recipient. We can then store all our email templates in a folder neatly making it super easy to edit.

In CodeIgniter we had a similar implementation we build into a helper that would pull views and allow us to send emails easily. But I can’t say it enough, it’s as though Laravel has just taken all the small hassles out of CodeIgniter and made life easier. All the little things that were constantly implemented on top of CodeIgniter come out of the box with Laravel.