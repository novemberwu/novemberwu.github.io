---
title: Lab 6
nav_order: 6
parent: Labs
layout: page

---
# Lab 6 Math
{: .no_toc}


## Goal
{: .no_toc}
* Able to use Math APIs

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Task A: Maximum value

Easy
{: .label .label-green }

Barring overflow, give a code fragment to compute the maximum of two integers a and b without using Math.max() or if.

```int max = (a + b + Math.abs(a - b)) / 2;```


## Task B: Area of a triangle

Easy
{: .label .label-green }
Write a program TriangleArea.java that takes three command line inputs a, b, and c, representing the side lengths of a triangle, and prints the area of the triangle using 

Heron's formula: ```area = sqrt(s(s-a)(s-b)(s-c)), where s = (a + b + c) / 2.```



