---
layout: default
title: Codility Lesson Two Counting Elements Missing Integer Solution
keywords: codility, lesson, two, counting, elements, missinginteger, missing, integer, solution
description: PHP solution for lesson two, counting elements, missing integer programming question.
date: Aug 14 2015
---

* [Link to test](https://codility.com/demo/take-sample-test/missing_integer)

## Solution

This one is pretty straight forward assuming we use the size of the array as the upper limit of `N`. This seems to pass all the tests so we will keep that assumption.

~~~
function solution($A)
{
    $arr = array_fill(0, count($A), null);
    
    foreach ($A as $v) {
        if ($v > 0) {
            $arr[$v] = true;
        }
    }
    
    for ($i = 1, $size = count($arr); $i < $size; $i++) {
        if ( ! isset($arr[$i])) {
            return $i;
        }
    }
      
    return $size;
}
~~~