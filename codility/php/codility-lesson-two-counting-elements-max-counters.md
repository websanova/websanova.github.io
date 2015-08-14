---
layout: default
title: "Codility Lesson Two :: Counting Elements :: Max Counters Solution"
keywords: codility, lesson, two, counting, elements, maxcounters, max, counters, solution
description: PHP solution for lesson two, counting elements, max counters programming question.
date: Aug 14 2015
---

* [Link to test](https://codility.com/demo/take-sample-test/max_counters)

## Easy Solution

I think I spent more time trying to understand this question than actually solving it. Pretty straight forward otherwise. However this one runs about slow because of the array fills.

~~~
function solution($N, $A)
{
    $max_curr = 0;
    $arr = array_fill(0, $N, 0);

    foreach ($A as $v) {
        if ($v >= 1 && $v <= $N) {
            $arr[$v - 1]++;
            
            if ($arr[$v - 1] > $max_curr) {
                $max_curr = $arr[$v - 1];
            }
        }
        else {
            $arr = array_fill(0, $N, $max_curr);
        }
    }
    
    return $arr;
}
~~~

## Efficient Solution

We already keep track of the `$max_curr` value so we can actually use that in real time. We can replace our fill with another `$max` value. Then in our loop before an increment we just just the max and update it. Finally at the end we will have to update any missing elements to the upper `$max` bound.

~~~
function solution($N, $A)
{
    $max = 0;
    $max_curr = 0;
    $arr = array_fill(0, $N, 0);

    foreach ($A as $v) {
        if ($v >= 1 && $v <= $N) {
            
            if ($max > $arr[$v - 1]) {
                $arr[$v - 1] = $max;
            }
            
            $arr[$v - 1]++;
            
            if ($arr[$v - 1] > $max_curr) {
                $max_curr = $arr[$v - 1];
            }
        }
        else {
            $max = $max_curr;
        }
    }
    
    foreach ($arr as $i => $v) {
        if ($v < $max) {
            $arr[$i] = $max;
        }
    }
    
    return $arr;
}
~~~