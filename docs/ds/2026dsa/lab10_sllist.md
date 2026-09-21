---
title: Lab 10 Single Linked List (SLList)
nav_order: 6
parent: 2026 DSA
layout: page
math: mathjax
---

# Lab 10 Single Linked List (SLList)
{: .no_toc}

## Goals:
{: .no_toc}
* Understand the concept of encapsulation and access control using `private` and `private static` helper nested classes.
* Learn how the `SLList` (Single Linked List) class acts as a wrapper that hides the raw node manipulation from the user.
* Contrast the usability and safety of `SLList` versus `IntList`.
* Implement the core prepending method `addFirst(int value)`.
* Understand how to retrieve the head element via `getFirst()`.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Introduction to SLList (Single Linked List)

In **Lab 9**, we built `IntList`, which directly exposed the `first` and `rest` variables to the client. While simple and elegant, `IntList` had a few major drawbacks:

1. **Naked Data Structure:** The user had to be highly aware of nodes, pointers, and `null` references when creating and manipulating lists (e.g., `new IntList(5, new IntList(10, null))`).
2. **Difficult APIs:** If the user wanted to add an element to the front of the list, they had to manually reassign references (e.g., `L = new IntList(3, L)`), which is error-prone and unintuitive.
3. **No Empty Lists:** An empty list could only be represented as `null`, which causes `NullPointerException` if any instance methods are called on it.

To solve these issues, we introduce the **SLList** (Single Linked List). The `SLList` is a **wrapper class** that encapsulates the raw node-based linked list structure.

```
       +------------------------------------+
       |              SLList                |  <--- Wrapper Class (User/Client interacts only with this)
       | - first: IntNode                   |
       +------------------------------------+
                         |
                         v
       +------------------------------------+
       |             IntNode                |  <--- Private Nested Helper Class (Hidden from the client)
       | + value: int                       |
       | + next: IntNode                    |
       +------------------------------------+
```

---

## Encapsulation & Nested Classes

One of the key tenets of Object-Oriented Programming (OOP) is **encapsulation**. The implementation details of how the data is stored should be completely hidden from the user.

To achieve this:
1. We make the `IntNode` class a `private static` nested class inside `SLList`. This means that code outside the `SLList` class cannot see or instantiate `IntNode` directly.
2. The field pointing to the head of our list, `first`, is declared as `private`. Only `SLList` methods can access or modify it.

```java
public class SLList {
    private static class IntNode {
        public int value;
        public IntNode next;
        public IntNode(int value, IntNode next){
            this.value = value;
            this.next = next;
        }
    }
    
    private IntNode first;
    ...
}
```

---

## Example A: The Complete SLList Implementation

Easy
{: .label .label-green }

Let's explore the code for our Single Linked List implementation. The wrapper class `SLList` simplifies list instantiation and prepending.

Create a file named `SLList.java` in your `Lists` package and implement the following:

```java
package Lists;

public class SLList {
    private static class IntNode {
        public int value;
        public IntNode next;
        public IntNode(int value, IntNode next){
            this.value = value;
            this.next = next;

        }
    }
    private IntNode first;
    public SLList(int value){
        this.first = new IntNode(value, null);
    }

    public void addFirst(int value){
        this.first  =new IntNode(value,this.first);

    }

    public int getFirst(){
        return this.first.value;
    }

    public static void main(String[] args) {
        SLList l = new SLList(10);
        l.addFirst(15);
        System.out.println(l.getFirst());

        l.addFirst(20);
        System.out.println(l.getFirst());
    }
}
```

---

## Example B: Analyzing the Mechanics of `addFirst`

Medium
{: .label .label-yellow }

Let's break down step-by-step how `addFirst` works in memory.

### Step 1: Initialization
When we construct a list using `SLList l = new SLList(10);`, a single `IntNode` is created in memory:

```
 l (SLList)
 +---------------+
 | first: -------> [ IntNode ]
 +---------------+ | value: 10
                   | next: null
```

### Step 2: Prepending 15
When we call `l.addFirst(15);`, the following expression is evaluated:
`this.first = new IntNode(15, this.first);`

1. A new `IntNode` is instantiated with value `15`.
2. Its `next` parameter is passed the current reference of `this.first` (which points to the node containing `10`).
3. `this.first` is updated to point to this newly created `IntNode`.

```
 l (SLList)
 +---------------+
 | first: -------> [ IntNode ]      [ IntNode ]
 +---------------+ | value: 15  ---> | value: 10
                   | next: ------/   | next: null
```

### Step 3: Prepending 20
Similarly, when we call `l.addFirst(20);`, the same steps are executed. The head of the list becomes `20`, which points to `15`, which in turn points to `10`.

```
 l (SLList)
 +---------------+
 | first: -------> [ IntNode ]      [ IntNode ]      [ IntNode ]
 +---------------+ | value: 20  ---> | value: 15  ---> | value: 10
                   | next: ------/   | next: ------/   | next: null
```

---

## Discussion Questions

To reinforce your understanding of encapsulation and `SLList`, think about and answer the following questions:

1. **Why is `IntNode` nested as `static` inside `SLList`?**
   * **Answer:** Since an `IntNode` does not need to access any instance variables or methods of its outer `SLList` instance, declaring it `static` saves memory because it doesn't need to hold a reference to the outer `SLList` object.
2. **What would happen if the user of `SLList` tried to do `SLList.IntNode n = new SLList.IntNode(5, null);` in their own class?**
   * **Answer:** It would result in a compilation error because `IntNode` is declared as `private` within `SLList`. This enforces encapsulation and prevents raw node exposure to clients.
