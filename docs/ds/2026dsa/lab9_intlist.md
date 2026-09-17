---
title: Lab 9 IntList and Linked Data Structures
nav_order: 5
parent: 2026 DSA
layout: page
math: mathjax
---

# Lab 9 IntList and Linked Data Structures
{: .no_toc}

## Goals:
{: .no_toc}
* Understand the concept of a **Dynamic Set** and the fundamental operations that define it.
* Contrast the core behaviors, pros, and cons of **Linked Lists** vs. **Array Lists**.
* Define a custom linked list class (`IntList`) using a nested node structure where objects point to other objects.
* Learn to build and traverse linked structures using both **recursion** and **iteration**.
* Implement and distinguish between **non-destructive** (immutable) and **destructive** (mutable, in-place) operations on lists.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Introduction to Dynamic Sets & Linked Lists

A **Dynamic Set** is a mathematical abstraction representing a collection of elements that continuously grows, shrinks, and updates its content over time. In contrast to a static array of fixed capacity, a dynamic set adapts its memory footprint as elements are added or removed.

### Everyday Examples of List Operations:
Every dynamic set data structure supports a core set of API methods:
* **Add:** Append or insert a new item into the collection.
* **Remove:** Delete a specified item and return its value (or update structure).
* **IsEmpty:** Test if the collection has zero elements.
* **Size:** Query and return the current count of elements in the collection.

```
       +---------------------------------------------+
       |             Dynamic Set API                 |
       |  - add(item)                                |
       |  - remove() -> item                         |
       |  - isEmpty() -> boolean                     |
       |  - size() -> int                            |
       +---------------------------------------------+
               /                             \
              v                               v
   +-----------------------+       +-----------------------+
   |      Array List       |       |      Linked List      |
   | (Contiguous memory,   |       | (Scattered nodes,     |
   |  resizes by doubling) |       |  linked by pointers)  |
   +-----------------------+       +-----------------------+
```

### Array List vs. Linked List
* **Array List:** Stores elements in sequential, contiguous slots of memory. Accessing any index is fast ($$O(1)$$), but inserting or deleting elements near the front requires shifting all subsequent elements, which is slow ($$O(N)$$). When the array is full, it must allocate a larger array and copy all items.
* **Linked List:** Consists of a sequence of independent node objects scattered throughout the heap. Each node contains a data element and a reference (or "pointer") to the next node. Inserting or deleting elements only requires updating reference variables ($$O(1)$$ once positioned), but accessing elements requires traversing the list from the beginning node by node ($$O(N)$$).

---

## The Concept of a Linked List

A **Linked List** is a recursive data structure. A non-empty list consists of:
1. A data value (`first`) of a primitive or object type.
2. A reference (`rest`) to another list. If the current node is the final element, its `rest` reference points to `null`.

Let's represent this recursive definition in Java by defining a class called `IntList`:

```java
public class IntList {
    public int first;
    public IntList rest;

    // Constructor to initialize a single node
    public IntList(int f, IntList r) {
        this.first = f;
        this.rest = r;
    }
}
```

Visually, a linked list with three items (`5`, `10`, and `15`) is linked together via reference pointers in the heap:

```
    L
    |
    v
+---------+      +---------+      +---------+
| first:5 |      | first:10|      | first:15|
| rest: -------> | rest: -------> | rest:null|
+---------+      +---------+      +---------+
```

---

## Example A: Node Construction and List Creation

Easy
{: .label .label-green }

Let's look at how to construct the linked list depicted above in code. There are two common approaches to building a linked list manually:

### 1. Manual Re-linking (The "Long" Way)
We can instantiate each node one by one and hook their references manually:

```java
// Create a file named IntList.java and add this main client code
public class IntList {
    public int first;
    public IntList rest;

    public IntList(int f, IntList r) {
        this.first = f;
        this.rest = r;
    }

    public static void main(String[] args) {
        // Construct the 1st node
        IntList L = new IntList(15, null);
        
        // Construct the 2nd node and link it from the front
        L = new IntList(10, L);
        
        // Construct the 3rd node and link it from the front
        L = new IntList(5, L);

        // Print values to verify linking
        System.out.println("Front value: " + L.first);               // Prints 5
        System.out.println("Second value: " + L.rest.first);          // Prints 10
        System.out.println("Third value: " + L.rest.rest.first);     // Prints 15
    }
}
```

### 2. Nested Constructors (The "Compact" Way)
Because our constructor takes the `rest` reference as an argument, we can nestedly declare constructors to construct the entire list in a single, elegant line of code:

```java
IntList L = new IntList(5, new IntList(10, new IntList(15, null)));
```

---

## Example B: Traversing the List (Recursive vs. Iterative Size)

Medium
{: .label .label-yellow }

To perform operations on a linked list, we must traverse through each node. Because linked lists are recursive by nature, we can write traversals very elegantly using recursion, or we can use standard loops to iterate over each pointer.

Let's study both approaches to compute the **size** of a list:

### 1. Recursive Size
The size of a linked list starting at a given node is:
* $$1$$ (for this current node) plus the size of the rest of the list.
* **Base Case:** If `rest` is `null`, then the size is simply $$1$$.

```java
public int size() {
    if (this.rest == null) {
        return 1;
    }
    return 1 + this.rest.size();
}
```

### 2. Iterative Size
We can traverse the list using a temporary pointer variable (historically named `p`) and a loop. We count the nodes until `p` becomes `null`.

```java
public int iterativeSize() {
    IntList p = this;
    int totalSize = 0;
    while (p != null) {
        totalSize += 1;
        p = p.rest;
    }
    return totalSize;
}
```

---

## Task A: Implementing `get(int i)`

Easy
{: .label .label-green }

Now, write a method that simulates random-access array indexing. Implement a method `get(int i)` that returns the value of the $$i$$-th element in the list.

### Instructions:
1. Open your `IntList.java` file.
2. Add a method with this exact signature: `public int get(int i)`
   * The front-most item of the list is considered the **0th item**.
   * For simplicity, you may assume that the element at index `i` exists (i.e., `0 <= i < size()`).
3. You can implement `get` using either **recursion** or an **iterative loop** (try both for practice!).
   * **Hint (Recursive approach):** The $$i$$-th element of list `L` is equivalent to the $$(i-1)$$-th element of list `L.rest`. What is the base case?
   * **Hint (Iterative approach):** Set a pointer `p = this;` and write a `for` or `while` loop that advances `p = p.rest;` exactly `i` times, then return `p.first`.
4. Run the code inside the main method below to verify your implementation.

### Expected Output:
```
Size of list L: 3
L.get(0) expected 5: 5
L.get(1) expected 10: 10
L.get(2) expected 15: 15
```

### Code Skeleton:
```java
public class IntList {
    public int first;
    public IntList rest;

    public IntList(int f, IntList r) {
        this.first = f;
        this.rest = r;
    }

    public int size() {
        if (this.rest == null) {
            return 1;
        }
        return 1 + this.rest.size();
    }

    // TODO 1: Implement get(int i) recursively or iteratively
    public int get(int i) {
        // Your code here
        return -1;
    }

    public static void main(String[] args) {
        // Construct the list: 5 -> 10 -> 15
        IntList L = new IntList(5, new IntList(10, new IntList(15, null)));

        System.out.println("Size of list L: " + L.size());
        System.out.println("L.get(0) expected 5: " + L.get(0));
        System.out.println("L.get(1) expected 10: " + L.get(1));
        System.out.println("L.get(2) expected 15: " + L.get(2));
    }
}
```

---

## Task B: Non-Destructive List Incrementing (`incrList`)

Medium
{: .label .label-yellow }

When performing modifications on linked lists, we must choose whether the operation is **destructive** or **non-destructive**:
* **Non-destructive operations** leave the original list completely unchanged. They create entirely **new** list nodes in memory with the updated values.

In this task, write a static method `incrList(IntList L, int x)` that returns a *new* list identical to `L`, but with all of its elements incremented by `x`. The original nodes of `L` **must not be modified**.

```
Original list L:  [ 5 | rest ] ---> [ 10 | rest ] ---> [ 15 | null ]

incrList(L, 3) returned list:
                  [ 8 | rest ] ---> [ 13 | rest ] ---> [ 18 | null ]
```

### Instructions:
1. Create a new file named `ExtraIntListPractice.java` (or add it directly below your `IntList` class inside `IntList.java` as a static helper method).
2. Add a static method with this exact signature:
   `public static IntList incrList(IntList L, int x)`
3. **Constraint:** You **cannot** modify the values of the input list `L`. You must instantiate new list nodes using `new IntList(...)` for each element in the returned list.
4. Implement this method **recursively**:
   * **Base Case:** If `L == null`, then return `null`.
   * **Recursive Step:** Return a newly constructed `IntList` node. Its `first` field should be `L.first + x`, and its `rest` reference should be the result of recursively calling `incrList` on `L.rest`.

### Expected Output:
```
Original List L (unchanged): 5 -> 10 -> 15
Incremented List (new list): 8 -> 13 -> 18
```

---

## Task C: Destructive List Incrementing (`dincrList`)

Hard
{: .label .label-red }

* **Destructive operations** modify the existing list directly in the heap. They **do not** allocate any new nodes using the `new` keyword. Instead, they edit the existing nodes' variables in-place.

In this task, write a static method `dincrList(IntList L, int x)` that increments all values of `L` by `x` **without using the `new` keyword**.

```
Original list L in heap:
[ 5 | rest ] ---> [ 10 | rest ] ---> [ 15 | null ]
       |                 |                  |
       v (mutate)        v (mutate)         v (mutate)
[ 8 | rest ] ---> [ 13 | rest ] ---> [ 18 | null ]
```

### Instructions:
1. Open `ExtraIntListPractice.java` or `IntList.java`.
2. Add a static method with this exact signature:
   `public static IntList dincrList(IntList L, int x)`
3. **Constraint:** You are **not allowed to use the `new` keyword** inside `dincrList`! You must overwrite the `first` instance variables of the input list's existing nodes.
4. You can implement this recursively or iteratively:
   * **Recursive approach:** If `L == null`, return `null`. Otherwise, add `x` to `L.first`, recursively call `dincrList` on `L.rest`, and finally return `L`.
   * **Iterative approach:** Walk down the list with a pointer `p = L;` and add `x` to `p.first` inside a `while (p != null)` loop, then return `L`.

---

## Verifying Lab 9 Work (Full Practice Script)

To verify your solutions for both **Task B** and **Task C**, implement the complete client test runner below. 

Create a file named `ExtraIntListPractice.java` in your workspace and use it to test your work:

```java
public class ExtraIntListPractice {

    /** 
     * Returns an IntList identical to L, but with all values incremented by x.
     * Non-destructive: L remains completely unchanged.
     */
    public static IntList incrList(IntList L, int x) {
        // TODO: Implement recursively using 'new'
        if (L == null) {
            return null;
        }
        return new IntList(L.first + x, incrList(L.rest, x));
    }

    /** 
     * Returns an IntList identical to L, but with all values incremented by x.
     * Destructive: modifies the existing nodes of L in-place. No 'new' allowed!
     */
    public static IntList dincrList(IntList L, int x) {
        // TODO: Implement iteratively or recursively (NO 'new' keyword)
        IntList p = L;
        while (p != null) {
            p.first += x;
            p = p.rest;
        }
        return L;
    }

    // Helper method to print IntList structures visually
    public static void printList(IntList L) {
        IntList p = L;
        while (p != null) {
            System.out.print(p.first);
            if (p.rest != null) {
                System.out.print(" -> ");
            }
            p = p.rest;
        }
        System.out.println();
    }

    public static void main(String[] args) {
        // Construct: 5 -> 10 -> 15
        IntList L = new IntList(5, new IntList(10, new IntList(15, null)));
        
        System.out.print("Original List L: ");
        printList(L);

        // Test Non-destructive incrList
        IntList L_incr = incrList(L, 3);
        System.out.print("After incrList(L, 3), new list: ");
        printList(L_incr);
        System.out.print("Original list L (should be unchanged): ");
        printList(L);

        System.out.println("----------------------------------------");

        // Test Destructive dincrList
        IntList L_dincr = dincrList(L, 3);
        System.out.print("After dincrList(L, 3), modified list: ");
        printList(L_dincr);
        System.out.print("Original list L (should be modified!): ");
        printList(L);
    }
}
```

### Expected Output:
```
Original List L: 5 -> 10 -> 15
After incrList(L, 3), new list: 8 -> 13 -> 18
Original list L (should be unchanged): 5 -> 10 -> 15
----------------------------------------
After dincrList(L, 3), modified list: 8 -> 13 -> 18
Original list L (should be modified!): 8 -> 13 -> 18
```

---

## Wrap-Up & Core Takeaways

Linked lists represent a huge paradigm shift from continuous memory structures like arrays:
1. **Recursion:** Linked structures align naturally with recursive algorithms. The base case is almost always reaching a node where `rest == null` or working with `null` lists.
2. **References as Links:** In Java, variables store references to objects, not the objects themselves. Mutating a reference (e.g., `p = p.rest;`) changes *where* the pointer is looking, while mutating a field (e.g., `p.first = p.first + x;`) changes the actual data residing in the heap.
3. **Memory management:** Destructive methods (`dincrList`) save memory by updating fields on the heap in-place, while non-destructive methods (`incrList`) are safer because they avoid unintended side effects across shared variables, but require allocating $$N$$ new node objects.
