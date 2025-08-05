---
title: Lab 3
nav_order: 3
parent: Labs
layout: page

---
# Lab 3 Expressions and Output

## Goals:
* Able to use arithmetic expression
* Able to use assignment expression
* Able to use print to write to standard output
* Able to use scanner to read from standard input


## Task A: 

Easy
{: .label .label-green }

Write a program that takes two integer command-line arguments x and y
and prints the Euclidean distance from the point (x, y) to the origin (0, 0).

Sample input
```
java distance 30 40
```

Sample output
```
50
```

## Task B:

Moderate
{: .label .label-purple }

Write a program that reads 2 integers from a file, then print out the sum of them into standard output

Sample input
```
java twoSum num.txt
```
Content of the num.txt file
```
88 90
```

Sample output
```
178
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