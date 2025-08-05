---
title: Lab 2
nav_order: 2
parent: Labs
layout: page

---
# Lab 2 Variables and Expressions

## Goals:
* Able to write program with using variables and primitive types
* Able to process double type with desired precision requirements

## Task A: 

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

## Task B

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

## Task C

Moderate
{: .label .label-purple }

Write a program that can distribute dollars among people. The program takes in 2 arguments.
First argument is the dollar amount.
Second argument is the number of people ( <= 10)
The program prints out distributed amount for each person. 
if the total amount can not be evenly divided by number of people, a random person takes the residual

{: .note } 
Adding up each people's fraction should get the total amount of money

Sample input
```
java distributeMoney 2.5 3
```
Sample output
```
0.8 0.8 0.9
```



