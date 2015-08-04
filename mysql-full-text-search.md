---
layout: default
title: MySQL Full-Text Search
keywords: mysql, fulltext, search, full, text, websanova
description: He was using a LIKE query with wild cards % to build his search. But this is actually not a good way to do search especially on large tables with lots of data.
date: July 25 2013
---

I was helping out a friend earlier today to setup a little search function on his website. He was using a `LIKE` query with wild cards `%` to build his search. But this is actually not a good way to do search especially on large tables with lots of data. It’s much more efficient to use an index and it’s actually quite simple to get something basic up and running on MySQL. I decided to write this brief beginner overview of doing search in MySQL which should help anyone get up and going. There are of course many things that can be tweaked and optimized based on exactly what it is you are trying to achieve but this should be a pretty nice starting point for those new to setting up a search on their websites or apps.

There are two basic ways to setup a textual search in MySQL, I’ll cover each briefly below.

## LIKE

One way to do a search is to just use `LIKE`. Using `LIKE` basically does a brute force matching and will take longer the bigger your table grows. This is by far the quickest and dirtiest method to get something up and running. A query using `LIKE` would look something like this:

~~~
SELECT *
FROM `table`
WHERE `column1` LIKE '%test%';
~~~

This will return all rows where `column1` contains the string “text” anywhere in the field. We can expand this to multiple keywords using the `OR` operator:

~~~
SELECT *
FROM `table`
WHERE 
    `column1` LIKE '%test%' OR 
    `column1` LIKE '%mighty%';
~~~

##FULLTEXT

There are two modes for `FULLTEXT` search which we’ll cover below but basically the idea is that you will be searching for keywords in an index rather than in the column itself. This makes for much faster searches as we can just look up words in an index and then find what rows that keyword points to. With `FULLTEXT` we’ll have to do a little bit of setup first. There are two things we’ll need to do:

First off we’ll need to make sure our table is using the `MyISAM` engine and not `InnoDB` as `FULLTEXT` only works with the former. This can be done with the following query:

~~~
ALTER TABLE  `table` ENGINE = MYISAM;
~~~

Then create an index on the columns we want to search in said table.

~~~
ALTER TABLE `table` ADD FULLTEXT  `table_search` (`column1`, `column2`);
~~~

And that’s it, we’re all setup for text searches on those columns in that table now.

## BOOLEAN MODE

There are two basic `FULLTEXT` search modes. One of them is `BOOLEAN`. There are three simple rules you will need to remember for this mode.

* Spaces are treated as an `OR` operator.
* It allows operators like `+`, `-` and `*`.
* There is no relevance for the keywords, just matches.

This mode is used if you just want to return any hits and aren’t too worried about relevance. It also allows you to use operators for instance using `+` means “must include” and using `-` means “must NOT include”. There is a full list of operators on the [MySQL Boolean Full-Text Searches](http://dev.mysql.com/doc/refman/5.5/en//fulltext-boolean.html) page that you can take a look at for more information. An example query would look like this:

~~~
SELECT *
FROM  `table`
WHERE
    MATCH(`column1`) AGAINST('test' IN BOOLEAN MODE);

SELECT *
FROM  `table`
WHERE
    MATCH(`column1`) AGAINST('+test -mighty' IN BOOLEAN MODE);
~~~

## NATURAL LANGUAGE MODE

The other mode is `NATURAL LANGUAGE` which only looks at the words themselves and tries to return matches based on relevance. This means the more times the words appear in the text the more relevant it is. This is different from `BOOLEAN` mode which uses the `OR` operand and just returns a row if it finds any hit for any of the words in the query. You can find more information on the [Mysql Natural Language Full-Text Searches](http://dev.mysql.com/doc/refman/5.0/en/fulltext-natural-language.html) page. An example for this mode would look like this:

~~~
SELECT *
FROM  `table`
WHERE
    MATCH(`column1`) AGAINST('test' IN NATURAL LANGUAGE MODE);

SELECT *
FROM  `table`
WHERE
    MATCH(`column1`) AGAINST('test mighty' IN NATURAL LANGUAGE MODE);
~~~

## WITH QUERY EXPANSION

There is an additional option for both modes for `QUERY EXPANSION`. All this does is expand the search to include relevant keywords to the keywords you have entered. So for instance if you search for “database” it might also match for keywords like “db”, “mysql”, “oracle”, “postgresql”. This will generally slow down your queries as it has to do another lookup for relevant keywords and then concatenate the results. Information on this mode can be found on the [MySQL Full-Text Searches with Query Expansion](http://www.websanova.com/blog/mysql/mysql-full-text-search) page. Using this extra bit is simple and can be done like so:

~~~
SELECT *
FROM  `table`
WHERE
    MATCH(`column1`) AGAINST('test' IN BOOLEAN MODE WITH QUERY EXPANSION);

SELECT *
FROM  `table`
WHERE
    MATCH(`column1`) AGAINST('test mighty' IN NATURAL LANGUAGE MODE WITH QUERY EXPANSION);
RELEVANCE
~~~

If you need both `RELEVANCE` and `BOOLEAN` mode then we can combine the queries together.

~~~
SELECT
    MATCH (`column1`) AGAINST ('some words' IN NATURAL LANGUAGE MODE) AS relevance
FROM `table`
WHERE
    MATCH (`column1`) AGAINST ('some words' IN BOOLEAN MODE)
ORDER BY relevance DESC
~~~

This will give us a quick `BOOLEAN` search and then order the results based on their relevance. This is typically the query I use unless `BOOLEAN` mode is not required. In that case we can just run both queries in `NATURAL LANGUAGE` mode.

## Performance

Overall you should see considerable performance improvement when using `FULLTEXT` over `LIKE` without any additional setup. Likewise with `QUERY EXPANSION` you will also see significant improvements. Keep in mind adding QUERY EXPANSION will slow us down a bit for the added lookups but not too much.

## Conclusion

In the end it depends what you’re trying to achieve. There are many things to tweak and adjust depending on what columns you want to search and how you want to order relevance. For the most part it’s best to use the `NATURAL` for `relevance` combined with `BOOLEAN` approach. This typically will run the fastest.