---
title: Lab 5
nav_order: 5
parent: Labs
layout: page

---
# Lab 5 Compound Assignment and Documentation
{: .no_toc}
## Goals
{: .no_toc}
* Able to process data type with desired precision requirements
* Able to use compound assignment
* Able to comment program to make it more readable

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Task A Compound interest

Moderate
{: .label .label-purple }

Continuously compounded interest. Write a program that calculates and
prints the amount of money you would have after t years if you invested P dollars
at an annual interest rate r (compounded continuously).

Please include comments whenever needed 

{: .note }
>The desired value is given by the formula $$E=Pe^{rt}$$
>
>You may need to use the Math lib in the calculation

## Task B Distribute money

Moderate
{: .label .label-purple }

Write a program that can distribute dollars among people. The program takes in 2 arguments.
First argument is the dollar amount.
Second argument is the number of people ( <= 10)
The program prints out distributed amount for each person.
if the total amount can not be evenly divided by number of people, a random person takes the residual

Please include comments whenever needed

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