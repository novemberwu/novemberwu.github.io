---
title: G10 Lab Loop
nav_order: 13
parent: Labs
layout: page

---
# Lab Loop 
{: .no_toc }
## Goals:
{: .no_toc }
* Able to use Loop

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}


## Task A: Math Problem
Moderate
{: .label .label-purple }

Draw a flowchart and write pseudocode for an interative algorithm that calculate
$$1+\frac{1}{2^{2}}+\frac{1}{3^{2}}+\frac{1}{4^{2}}+ ... + \frac{1}{n^{2}}$$

For any given n, an interesting mathematical fact:  these sums converge to $$\frac{\pi^{2}}{6}$$ as n increases.

Write a program to see how close is the sum for n = 10000 to $$\frac{\pi^{2}}{6}$$ 


## Task B: Math Problem 2

Moderate
{: .label .label-purple }

Draw a flowchart and write pseudocode for an interative algorithm that calculate
$$1-\frac{1}{2}+\frac{1}{3}-\frac{1}{4}+ ... +(or -) \frac{1}{n}$$

For any given n, an interesting mathematical fact:  these sums converge to $$ln2$$ as n increases.
**Maclaurin series expansions**

$$ln(1+x) = x-\frac{x^{2}}{2}+\frac{x^{3}}{3}-\frac{x^{4}}{4}+ ... +(or -) \frac{x^{n}}{n}$$

$$where(-1 \lt x \le 1)$$  

Write a program that compare the sum for n=10000 with the value returned by ```Math.log(2)```



