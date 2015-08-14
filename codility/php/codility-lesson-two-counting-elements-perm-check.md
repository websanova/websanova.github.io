---
layout: default
title: "Codility Lesson Two :: Counting Elements :: Perm Check Solution"
keywords: codility, lesson, two, counting, elements, permcheck, perm, check, solution
description: PHP solution for lesson two, counting elements, perm check programming question.
date: Aug 14 2015
---

* [Link to test](https://codility.com/demo/take-sample-test/perm_check)

## Solution

Since we are dealing with numbers from `1 - N` it makes it easy as we can just compare it to size. Then we also check for duplicates. We fail out if either case fails and return the passing value at the end if we get through the loop successfully.

~~~
function solution($A)
{
    $size = count($A);
    
    foreach ($A as $v) {
        if ($v > $size || isset($arr[$v])) {
            return 0;
        }
        
        $arr[$v] = true;
    }
    
    return 1;
}
~~~