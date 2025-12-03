---
title: Double Linked List Lab
nav_order: 11
parent: DS Labs
layout: page

---
# Double Linked List Lab
{: .no_toc}


## Goal
{: .no_toc}
* Gain un understanding of the usage of a data structure backing linked list
* Have experience with building a middle-size project from the scratch

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Delivery
Your delivery is the java file. Please make sure uploading the source files (.java) files.

## Software Engineering Philosophy
{: .no_toc}
A common beginner mistake in software engineering is to write a large amount of code 
and hope that it all work once you've finished. This makes life very difficult for a programmer.

A better approach is breaking down a whole piece of program into many smaller parts. Each time I make a 
baby step by modifying a small part, then verify that small piece works as expected. then go another baby step, so on and on.

To help encourage better programming habits, in this lab, we are going to hold your hands through the development
process. 

{: .note }
For this lab, you must work alone. Please do not use ```java.util``` data structures
in your implementation. The whole point is to build your own version of linked list

## Task A: Set up code skeleton (5% ponits)
Follow the [Guide](https://github.com/XiaochuanQian/PostAPDS-Lab-doubleLinkedList) to get the
code skeleton. The code skeleton defines the interfaces of Double Linked List

## Task B: Write and verify ```addFirst``` and ```addLast``` (25% points)
```addFirst``` and ```addLast``` *may not* use looping or recursion. A single add operation must take "constant time"
that is, adding an element should take approximately the same amount of time regardless the length of the list.

you can use the debugger or java visualizer to verify that your code is working correctly

## Task C: Write and verify ```getFirst``` and ```getLast``` (25% points)
Those are operations to return the value of the first node and last node of the list

## Task D: Write and verify constructors ```DLList()``` and ```DLList(T x)``` (10% points)
As  constructor ```DLList()``` will make an empty list, you might want to revise your implementation to make all methods
null safe.

## Task E: Write and verify ```removeFirst()``` and ```removeLast()``` (25% points)
```removeFirst``` and ```removeLast``` *may not* use looping or recursion. A single remove operation must take "constant time"
that is, removing an element should take approximately the same amount of time regardless the length of the list.

## Task F: Write ```size``` (5% points)
size is the number of nodes in the list. if you use sentinel nodes, they don't count. 
Because they are the implementation detail, not something we should expose to the user

## Task G: Write and verify ```get()``` (5% points)
You may use the loop or recursion to implement get, as there is no constant time implementation.

## Task H: Adding Test Cases ( Optional 10% points)
Write test cases and show the test coverage of your source. Please make sure test cases contain 3 parts
1. Precondition, or setup for the test
2. Execution of the to-be-tested functions
3. Assertion on expected result and actual result
