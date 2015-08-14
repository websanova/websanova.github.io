---
layout: default
title: Codility Lesson Two Counting Elements Frog River One Solution
keywords: codility, lesson, two, counting, elements, frogriverone, frog, river, one, solution
description: PHP solution for lesson two, counting elements, frog river one programming question.
date: Aug 14 2015
---

* [Link to test](https://codility.com/demo/take-sample-test/frog_river_one)

## Solution

Pretty straight forward. Just don't forget the part that says you need two wait for each leaf on the way to fall before the frog can make it all the way to the end.

~~~
function solution($X, $A)
{
    $arr = [];

    foreach ($A as $k => $v) {
        if ( ! isset($arr[$v])) {
            $arr[$v] = true;
        }
        
        if (count($arr) === $X) {
            return $k;
        }
    }
    
    return -1;
}
~~~
