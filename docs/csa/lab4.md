---
title: Lab 4
nav_order: 4
parent: Labs
layout: page

---
# Lab 4 Type Casting and Variable Range
{: .no_toc}
## Goals
{: .no_toc}
* Aware of the variable range and able to avoid overflow problem 
* Able to write type casting 

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Task A Compare doubles

Easy
{: .label .label-green }

The following expression ```(Math.sqrt(2) * Math.sqrt(2)) == 2``` evalutes to ```false```. 

Please write a program to reflect the desired result $$\sqrt2 * \sqrt2 = 2$$

```
public static void main(String[] args){
    System.out.println(/*your expression*/);
}

// the main function should print true
   
```
## Task B Overflow

Easy
{: .label .label-green }
Write a program to get the absolute value of -2,147,483,648

```
int min_value = Integer.MIN_VALUE;
// your code to process the min_value
// finally print out the absolute value, which is 2147483648
```

{: .note }
You will be surprised to the result of ```Math.abs(-2147483648)```