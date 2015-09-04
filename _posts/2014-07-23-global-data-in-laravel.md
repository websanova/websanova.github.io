---
layout: master
title: Global Data in Laravel
keywords: laravel, ioc, facade, service, provider, facades, services, data
description: Setting up a global Data store in Laravel for easy use in Controllers, Models and Views.
date: Jul 23 2014
img: /wp-content/uploads/2014/07/global-data-in-laravel.png
permalink: /blog/laravel/global-data-in-laravel.html
---

I’ve been using Laravel for a while now and one thing that has always bothered me is that there isn’t any kind of global “data” store to use in requests. A typical setup is to pass data into the view by setting the layout in the `BaseController` and using `$this->layout->with()` in our controller methods.

~~~
class BaseController extends Controller {

    protected function setupLayout()
    {
        if (!is_null($this->layout)) {
            $this->layout = View::make($this->layout);
        }
    }
}

class HomeController extends BaseController {

    public function index()
    {
        $data = array(
            'title' => 'Home',
            'cities' => Cities::get()
        );

        $this->layout->with($data);
    }
}
~~~

Sometimes we will want to set data in our `BaseController` or `__construct` so we will create a more global object for our data and store it in `$this`.

~~~
class HomeController extends BaseController {

    public function __construct()
    {
        $this->data['cities'] = Cities::get();
    }

    public function index()
    {
        $this->data['title'] = 'Home';

        $this->layout->with($this->data);
    }
}
~~~

This is a little bit better but the problem with putting values into the `$this` object is that we won’t have access to the controllers `$this` inside models, filters or views. We need something a little more flexible and not as redundant as having to set `$this->layout->with($this->data)` all over the place.

To get this global `data` access we can take advantage of Laravel’s `IoC` container and build a simple little service for storing our data. Pretty much all we need is a simple `set` and `get` method and since we can set up our service as a singleton it’s just one object being used per request which is pretty much the same as using `$this->data` but much cleaner and more flexible.

Here is an example of the `Data` class:

~~~
// Data.php

namespace Myproject;

class Data
{
    public $data = [];

    public function get($key)
    {
        return isset($this->data[$key]) ? $this->data[$key] : null;
    }

    public function set($key, $val)
    {
        $this->data[$key] = $val;
    }

    public function isEmpty($key)
    {
        return empty($this->data[$key]);
    }
    
    public function all()
    {
        return $this->data;
    }
}
~~~

You can see we have four simple methods here. The two main ones being the `set` and `get` method and another frequently used one being the `isEmpty` method. Finally the `all` method in case we just want to dump the entire `data` object.

And of course the DataServiceProvider and DataFacade:

~~~
// DataServiceProvider.php

namespace Myproject;

use Illuminate\Support\ServiceProvider;

class DataServiceProvider extends ServiceProvider
{
    public function register() {
        $this->app->bind('MyprojectData', function() {
            return new Data;
        });
    }
}

// DataFacade.php

namespace Myproject;

use Illuminate\Support\Facades\Facade;

class DataFacade extends Facade
{
    protected static function getFacadeAccessor() { 
        return 'MyprojectData';
    }
}
~~~

We can then go ahead and make calls like so:

~~~
Data::set('title', 'Home');
Data::set('cities', Cities::get());

Data::isEmpty('title');

Data::get('cities');
Data::all();
~~~

What’s great about this setup is that we can add whatever convenience methods we like to the `Data` class to make life a little simpler. For instance if we wanted our data to come out as an array.

~~~
public function asArray($key)
{
    if (!$this->isEmtpy($key)) {
        if (is_array($this->get($key)) {
            return $this->get($key);
        }
        else {
            return array($this->get($key));
        }
    }
}
~~~

Now that we have our `Data` service in the `IoC` container it let’s us access our data from anywhere in Laravel including our models and filters. It avoid us having to pass data around through methods which can get annoying and redundant quite fast.

We can also use the `Data` service in our views directly like any other variable using `{{ "{{Data::get('title')"}}}}` for example. This last step is great as it means we can avoid all the `$this->layout->with()` calls. Unless you like having `{{ "{{$title"}}}}` variables directly in which case you can still do `$this->layout->with(Data::all())`.