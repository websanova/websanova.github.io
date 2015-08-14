---
layout: default
title: Codility Lesson One Time Complexity Perm Missing Elem Solution
keywords: codility, lesson, one, time, complexity, permmissingelem, perm, missing, elem, element, solution
description: PHP solution for lesson one, time complexity, perm missing elem programming question.
date: Aug 13 2015
---

* [Link to test](https://codility.com/demo/take-sample-test/perm_missing_elem)

## Solution

Pretty straight forward. We use the [nth triangular number](https://en.wikipedia.org/wiki/Triangular_number) equation (or `termial` number) to get sum of all numbers from `1 to N+1`. It's like factorial except for addition.

At that point we can just get the sum of the array and subtract the difference to get our missing number.

~~~
function solution($A)
{
    $n = sizeof($A) + 1;
    $termial = $n * ($n + 1) / 2;
    
    return $termial - array_sum($A);
}
~~~