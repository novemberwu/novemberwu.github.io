---
title: List Lab
nav_order: 9
parent: DS Labs
layout: page

---
# List Lab
{: .no_toc}


## Goal
{: .no_toc}
This lab aims to build deep understandings of the list data structure
* Able to use java built-in ArrayList and LinkedList
* Able to implement your own version of List with add/remove/isFull functions

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Task 1.A: Implement **isFull** method for ArrayList
Easy
{: .label .label-green }

Use java built-in collection ArrayList class to build your own Array List Type. 
- The new array list will have a capacity property, where the capacity is a fixed size.
- implement the ```isFull``` method, it returns true when all slots are used out, and false otherwise

## Task 1.B: Implement **add** method for ArrayList
Moderate
{: .label .label-purple }
Use java built-in ```array``` to build your own Array List Type.
- The new array list will have a capacity property, where the capacity is a fixed size.
- The new array list will provide  ```add``` method. if the list is full, your method should throw exceptions with 
meaningful error messages. Otherwise, add the new element to the tail of the list
- You might want to use the following code in your linked list declaration

## Task 1.C Implement **remove** method for LinkedList
Moderate
{: .label .label-purple }
- Please reuse the task 1.B data structure
- The new array list will provide  ```remove``` method. if the list is empty, your method should throw exceptions with
  meaningful error messages. Otherwise, the element at the tail of the list should be removed

---

## Task 2.A: Implement **isFull** method for LinkedList
Easy
{: .label .label-green }
Use java built-in collection LinkedList class to build your own Linked List Type.
- The new linked list will have a capacity property, where the capacity is a fixed size.
- implement the ```isFull``` method, it returns true when all slots are used out, and false otherwise

## Task 2.B: Implement **add** method for LinkedList
Moderate
{: .label .label-purple }
Use **your own list data structure** to build your own Linked List Type.
- The new linked list will have a capacity property, where the capacity is a fixed size.
- The new linked list will provide  ```add``` method. if the list is full, your method should throw exceptions with
  meaningful error messages. Otherwise, add the new element to the **tail** of the list
- You might want to use the following code in your linked list declaration
```java
class IntNode {
        public int number;
        public IntNode next;

        public IntNode(int x, IntNode head) {
            this.number = x;
            this.next = head;
        }
    }
```

## Task 2.C: Implement **remove** method for LinkedList
Moderate
{: .label .label-purple }
- Please reuse the task 2.B data structure
- The new linked list will provide  ```remove``` method. if the list is empty, your method should throw exceptions with
  meaningful error messages. Otherwise, the element at the tail of the list should be removed


