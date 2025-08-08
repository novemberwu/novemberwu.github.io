---
title: Lab 8
nav_order: 8
parent: Labs
layout: page

---
# Lab 8 String Manipulation
{: .no_toc }
## Goals:
{: .no_toc }
* Able to use String APIs

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}


## Task A: Palindrome
Moderate
{: .label .label-purple }

Write a function that takes a string as input and determines whether it is a palindrome. 

A palindrome is a word, phrase, or sequence that reads the same backward as forward, ignoring spaces, punctuation, and capitalization

* The input string can contain uppercase and lowercase letters, digits, spaces, and punctuation marks.
* Consider only alphanumeric characters for palindrome checking.
* The function should return a boolean value (true if it's a palindrome, false otherwise).

Samples
```
Input: "A man, a plan, a canal: Panama"
Output: true

Input: "race a car"
Output: false
```


{: .note }
Utilize String methods like toLowerCase(), replaceAll(), charAt(), or substring() to preprocess the string and then compare characters from both ends