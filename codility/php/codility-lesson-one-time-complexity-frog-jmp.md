---
layout: default
title: Codility Lesson One Time Complexity Frog Jmp Soluion
keywords: codility, lesson, one, time, complexity, frogjmp, frog, jmp, jump, element, solution
description: PHP solution for lesson one, time complexity, frog jmp programming question.
date: Aug 13 2015
---

* [Link to test](https://codility.com/demo/take-sample-test/frog_jmp)

## Solution

This one is a little too easy. I suppose it tests your ability on some basic math and knowing how to cast a value.

~~~
function solution($X, $Y, $D) {
    return (int) ceil( ($Y - $X) / $D);
}
~~~