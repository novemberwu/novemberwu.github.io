---
title: Lab 2
nav_order: 2
parent: Labs
layout: page
math: mathjax


---
# Lab 2 Variables and Data Types
{: .no_toc}
## Goals:
{: .no_toc}
* Able to write program with using variables and primitive types

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Task A: Integers and Boolean

Easy
{: .label .label-green }

Write a program that takes two positive integers as command-line
arguments and prints true if either evenly divides the other.

Sample Input
```
java TwoDivide 2 4
java TwoDivide 7 3
```
Sample Output
```
true
false
```
Code Skeleton (you can copy this to your lab project)
```java
public class TwoDivide {

    public static void main(String[] args){
        // validations on user input
        if(args.length != 2) return;
        if("0".equals(args[0]) || "0".equals(args[1]))return;

        // convert user input from String to int
        int a = Integer.parseInt(args[0]);
        int b = Integer.parseInt(args[1]);
        //TODO: Your code here

    }
}
```

## Task B Operators 

Easy
{: .label .label-green }

Write a program that takes three positive integers as command-line
arguments and prints false if any one of them is greater than or equal to the sum
of the other two and true otherwise.
{: .note }
This computation tests whether the
three numbers could be the lengths of the sides of some triangle.

Sample input
```
java ThreeNumber 3 4 5
java ThreeNumber 9 20 8
```
Sample output
```
true
false
```
Code Skeleton (you can copy this to your lab project)

```java
public class ThreeNumber {
    public static void main(String[] args){
        // validations on user input
        if(args.length != 3) return;
        // convert user input from String to int
        int a = Integer.parseInt(args[0]);
        int b = Integer.parseInt(args[1]);
        int c = Integer.parseInt(args[2]);

        // TODO: your implementation here
        // step 1: check all inputs are positive numbers
        // step 2: check if the three number can form triangle
        // i.e. a + b > c, a + c > b, and b + c > a

    }
}
```






