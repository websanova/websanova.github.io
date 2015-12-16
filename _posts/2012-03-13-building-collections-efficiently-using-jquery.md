---
layout: master
title: Building Collections Efficiently Using jQuery
keywords: collection, efficient, jquery
description: The article covers building collections efficiently using jQuery, particularly the clone function.
date: Mar 13 2012
permalink: /blog/jquery/building-collections-efficiently-using-jquery.html
redirect_from:
  - /tutorials/jquery/building-collections-efficiently-using-jquery.html
  - /building-collections-efficiently-using-jquery.html
---

One issue that is commonly faced in an AJAX based website is the building of a collection or list of items.  The collection is dynamic meaning items can be added or removed and this is done so via AJAX calls.  The goal is to keep the JavaScript and HTML components separate while keeping the code understandable and maintainable.  There are a few frameworks out there to do this for you like the more popular `backbone.js` however I wanted to present a simpler solution without the use of any frameworks.

### The Problem

We have some type of collection of common items that can be added or removed on the fly via AJAX calls and no page refreshes.

~~~
<div id="blurbs_table"></div>

<div id="blurbs_table_items" style="display:none;">
    <div class="item">
        <div class="blurb">jQuery Rocks</div>
        <div class="menu"></div>
    </div>

    <div class="button view">view</div>
    <div class="button buy">buy</div>
    <div class="button info">info</div>
    <div class="button remove">remove</div>
</div>
~~~

### The Solution

Here are a few assumptions we are going to make for this solution:

- The data we are receiving is in JSON format.
- We want to keep our HTML and JavaScript code separate.
- Our items have dynamic components, for example the type of buttons in a menu (view, buy, sell, info, etc...)

~~~
<script type="text/javascript">
    function addItem()
    {
        //AJAX call to server, get back some JSON response
        var response = {item: {blurb: "This is a great example!"}}

        appendItem(response.item);
    }

    function removeItem(item)
    {
        //AJAX call to server, get back some JSON
        item.remove();
    }

    function loadItems()
    {
        //AJAX call to server, get JSON array of items
        var items = response.items;

        for(var item in items) appendItem(items[item]);
    }

    function appendItem(item)
    {
        var builder = $('#blurbs_table_items');
        var newItem = builder.children('.item').clone();
        var remove = builder
        .children('.button.remove')
        .clone()
        .click(function(){ removeItem($(this).parents('.item')); });

        newItem.children('.blurb').html(item.blurb);
        newItem.children('.menu').append(remove);
        newItem.appendTo('#blurbs_table');
    }
</script>
~~~

### Notes

- The real magic here happens with the jQuery clone() function allowing us to easily make copies of the components we need for our collection.
- We can use the loadItems() function to build our initial collection by just traversing through the list of items and appending them.
- Your functions that do things like add, remove or update would pretty much stay the same way.  They make an AJAX call get back some kind of response that if successful performs some action.  The important piece is the appendItem() function, here is where we build the items for our collection.
- Notice that we have hidden the "blurbs_table_items" as this contains all pieces for our collection.  Now we can update our HTML and JavaScript in their proper locations with no mashing together.  This makes it much easier to maintain, update and understand the code.
- This method is also particularly useful when you have many such collections similar to each other but with slight variations.  It gives you maximum reusability but keeps things from getting confusing.
- This also keeps our CSS code clean as we can write the full rules for one complete collection and they are only applied if the items within it exist.