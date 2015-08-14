---
layout: default
title: Codility Lesson One Time Complexity Tape Equilibrium Solution
keywords: codility, lesson, one, time, complexity, tapequilibrium, tape, equilibrium, solution
description: PHP solution for lesson one, time complexity, tape equilibrium programming question.
date: Aug 13 2015
---

* [Link to test](https://codility.com/demo/take-sample-test/tape_equilibrium)

## Lazy Solution

The first thing that comes to mind is to just use `array_sum` and `array_slice`. This will loop through but will perform a lot of sum and slice calculations. We can make it a bit more efficient quite easily.

~~~
function solution($A) {
    $min = null;
    $size = sizeof($A);
    
    for ($i = 1; $i < $size; $i++) {
        $diff = abs(array_sum(array_slice($A, 0, $i)) - array_sum(array_slice($A, $i, $size)));
    
        if ($min === null || $diff < $min) {
            $min = $diff;
        }
    }
    
    return $min;
}
~~~

## Efficient Solution

A more efficient solution is to just to preset totals for `front` and `back` and then subtract and add the current number in the array. This will avoid the `array_slice` and `array_sum` each time.

~~~
function solution($A) {
    $size = sizeof($A);
    
    $front = $A[0];
    $back = array_sum(array_slice($A, 1, $size));
    $diff = 0;
    $min = abs($front - $back);
    
    for ($i = 1; $i < $size; $i++) {
        $front += $A[$i];
        $back -= $A[$i];
        $diff = abs($front - $back);
    
        if ($diff < $min) {
            $min = $diff;
        }
    }
    
    return $min;
}
~~~