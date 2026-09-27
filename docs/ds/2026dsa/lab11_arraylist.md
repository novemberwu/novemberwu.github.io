---
title: Lab 11 Array-Based List (AList)
nav_order: 7
parent: 2026 DSA
layout: page
math: mathjax
---

# Lab 11 Array-Based List (AList)
{: .no_toc}

## Goals:
{: .no_toc}
* Understand the core motivation for an **Array-Based List (`AList`)** compared to Linked Lists (`SLList`, `DLList`).
* Appreciate how **contiguous memory allocation** enables constant-time ($$O(1)$$) arbitrary retrieval (`get(int i)`), known as **random access**.
* Master the concept of **Data Structure Invariants** in array-backed lists:
  * `size` always tracks the number of valid items in the list.
  * The next item added to the back (`addLast`) is placed at index `size`.
  * The last item in the list is always at index `size - 1`.
  * The first item in the list is always at index `0` (when `size > 0`).
* Analyze the lecture implementation of back operations: `addLast()`, `getLast()`, and `removeLast()`.
* Implement the front operations: **`addFirst(int x)`** and **`removeFirst()`**.
* Master array element **shifting mechanics** and understand why loop direction is critical to avoid data clobbering.
* Contrast the runtime performance of front vs. back operations in arrays: $$O(1)$$ back operations vs. $$O(N)$$ front operations.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## 1. Motivation: A Last Look at Linked Lists

In previous labs, we explored linked list structures:
* In **Lab 9 (`IntList`)**, we built naked recursive node links (`first` and `rest`).
* In **Lab 10 (`SLList`)**, we introduced encapsulation with a wrapper class and a private `IntNode`.
* In **Doubly Linked Lists (`DLList`)**, we used prev/next pointers and circular sentinels to make all front and back operations lightning fast:
  * `addFirst`, `addLast` $$\rightarrow O(1)$$
  * `getFirst`, `getLast` $$\rightarrow O(1)$$
  * `removeFirst`, `removeLast` $$\rightarrow O(1)$$

### The Linked List Bottleneck: Arbitrary Retrieval (`get`)

Suppose we want to implement `get(int i)`, which returns the item at index `i` (where 0 is the front):

```
sentinel <---> [ Node 0 ] <---> [ Node 1 ] <---> ... <---> [ Node i ] <---> ...
```

In any linked list, node objects are scattered across memory in the heap. There is no formula to jump directly to the $$i$$-th node; we **must scan through the list pointer-by-pointer**:
* To reach index `i`, we must traverse $$i$$ node references starting from the sentinel.
* For arbitrary $$i$$, `get(int i)` takes **$$O(N)$$ linear time**.

### Random Access in Arrays

Arrays in Java reside in a **single contiguous block of physical memory**. When you write `items[i]`, the Java runtime computes the memory address with simple arithmetic in constant time:

$$\text{Address of } items[i] = \text{Base Address} + (i \times \text{Size of Element})$$

Because calculating this address does not depend on the index $$i$$ or the array size, array retrieval is **$$O(1)$$ constant time**—no matter how large the list grows!

| Operation | Linked List (`DLList`) | Array-Based List (`AList`) |
| :--- | :---: | :---: |
| `addFirst` | $$O(1)$$ | **$$O(N)$$ (requires shifting)** |
| `addLast` | $$O(1)$$ | **$$O(1)$$** |
| `getFirst` | $$O(1)$$ | **$$O(1)$$** |
| `getLast` | $$O(1)$$ | **$$O(1)$$** |
| `removeFirst` | $$O(1)$$ | **$$O(N)$$ (requires shifting)** |
| `removeLast` | $$O(1)$$ | **$$O(1)$$** |
| **`get(int i)`** | **$$O(N)$$** | **$$O(1)$$ (Random Access!)** |

Our goal today is to build **`AList.java`**—a list backed by an underlying fixed array.

---

## 2. Naive AList Architecture & Invariants

An `AList` wraps a primitive array and an integer tracking the number of active elements:

```java
public class AList {
    private int[] items;
    private int size;

    public AList() {
        items = new int[100]; // initial capacity
        size = 0;
    }
}
```

```
items: [  0 ,  0 ,  0 ,  0 ,  0 ,  0 , ... ,  0  ]
index:    0    1    2    3    4    5           99
size:  0
```

### The Invariants of AList
An **invariant** is a property of a data structure that must hold true before and after every public method call:
1. **`size`:** Always reflects the exact count of items currently stored in the `AList`.
2. **`addLast` insertion point:** The next item to insert via `addLast` always goes into index `size`.
3. **`getLast` location:** The last valid item in the list is always at index `size - 1` (when `size > 0`).
4. **Front location:** The first item in the list is always at index `0` (when `size > 0`).

---

## 3. Lecture Recap: Back Operations

In lecture, we implemented the back and query operations:

### 1. `addLast(int x)`
To append an element to the back:
* Place `x` at `items[size]`.
* Increment `size` by 1.

```java
public void addLast(int x) {
    items[size] = x;
    size += 1;
}
```

### 2. `getLast()` and `get(int i)`
Because array access is $$O(1)$$:
```java
public int getLast() {
    return items[size - 1];
}

public int get(int i) {
    return items[i];
}

public int size() {
    return size;
}
```

### 3. `removeLast()`
Consider an `AList` with values `{5, 3, 1, 7, 22, -1}` (`size = 6`):
```
items: [  5 ,  3 ,  1 ,  7 , 22 , -1 ,  0 , ... ]
index:    0    1    2    3    4    5    6
size:  6
```
To remove the last item (`-1`):
1. Save `items[size - 1]`.
2. Decrement `size` (`size = 5`).
3. Return the saved item.

```java
public int removeLast() {
    int x = getLast();
    size -= 1;
    return x;
}
```

{: .note }
**Do we need to zero out `items[size]` in `removeLast()`?**  
No! As long as our invariants hold, users can only access elements from index `0` to `size - 1`. The value remaining at `items[5]` is effectively dead memory and will simply be overwritten on the next `addLast`.

---

## 4. The Lab Challenge: Front Operations

In lecture, we deliberately omitted **`addFirst`** and **`removeFirst`**. Why?  
Because in an array where the first element is pinned to index `0`, operations at the front **require shifting all existing elements**!

---

### Mechanics of `addFirst(int x)`

Suppose our list contains 4 elements: `{10, 20, 30, 40}` (`size = 4`), and we execute `addFirst(99)`:

```
Initial state:
items: [ 10 , 20 , 30 , 40 ,  0 ,  0 , ... ]
index:    0    1    2    3    4    5
size:  4
```

We must shift all elements one slot to the right so index `0` becomes available for `99`.

#### ⚠️ The Shifting Trap: Which direction should we loop?

**Wrong way (Left to Right):**
```java
// BUG! DO NOT DO THIS!
for (int i = 0; i < size; i++) {
    items[i + 1] = items[i];
}
```
* `i = 0`: `items[1] = items[0]` $$\rightarrow$$ `items[1]` becomes `10`!
* `i = 1`: `items[2] = items[1]` $$\rightarrow$$ `items[2]` becomes `10`!
* `i = 2`: `items[3] = items[2]` $$\rightarrow$$ `items[3]` becomes `10`!
* **Result:** Every slot is overwritten with `10`! The original data is destroyed!

**Correct way (Right to Left):**  
We must shift from the **back to the front**:
1. Copy `items[3]` to `items[4]`.
2. Copy `items[2]` to `items[3]`.
3. Copy `items[1]` to `items[2]`.
4. Copy `items[0]` to `items[1]`.
5. Now index `0` is free: set `items[0] = 99`.
6. Increment `size` to `5`.

```
Step 1: items[4] = items[3]  --> [ 10 , 20 , 30 , 40 , 40 , ... ]
Step 2: items[3] = items[2]  --> [ 10 , 20 , 30 , 30 , 40 , ... ]
Step 3: items[2] = items[1]  --> [ 10 , 20 , 20 , 30 , 40 , ... ]
Step 4: items[1] = items[0]  --> [ 10 , 10 , 20 , 30 , 40 , ... ]
Step 5: items[0] = 99        --> [ 99 , 10 , 20 , 30 , 40 , ... ]
Step 6: size += 1            --> size = 5
```

---

### Mechanics of `removeFirst()`

Suppose our list contains `{99, 10, 20, 30, 40}` (`size = 5`), and we call `removeFirst()`:
* The first element to return is `99` (`items[0]`).
* Every remaining element must slide one slot to the **left**:
  * Copy `items[1]` to `items[0]`.
  * Copy `items[2]` to `items[1]`.
  * Copy `items[3]` to `items[2]`.
  * Copy `items[4]` to `items[3]`.
* Decrement `size` to `4`.
* Return the saved `99`.

#### Loop direction for `removeFirst()`:
Here, we must loop from **left to right** (`i = 1` up to `size - 1`):
```java
items[i - 1] = items[i];
```
This safely pulls values forward into the vacated slot without overwriting any uncopied data.

---

## 5. Lab Tasks

### Task A: Inspect the Skeleton Code
Medium
{: .label .label-yellow }

Create a file named `AList.java` in your IDE and paste the skeleton code provided below. Notice how the back operations (`addLast`, `removeLast`, `getLast`, `get`, `size`) are already provided.

### Task B: Implement `addFirst(int x)`
Hard
{: .label .label-red }

Implement the `public void addFirst(int x)` method:
1. Verify if the list is full (`size == items.length`). If so, throw an `IllegalStateException("List is full")`.
2. Shift all existing elements from index `size - 1` down to `0` one spot to the right (`items[i + 1] = items[i]`).
3. Insert `x` into `items[0]`.
4. Increment `size` by 1.

### Task C: Implement `removeFirst()`
Hard
{: .label .label-red }

Implement the `public int removeFirst()` method:
1. Check if the list is empty (`size == 0`). If so, throw a `java.util.NoSuchElementException("Cannot remove from an empty list")`.
2. Store the first element (`items[0]`) in a temporary variable.
3. Shift all subsequent elements one spot to the left (from index `1` to `size - 1`).
4. Decrement `size` by 1.
5. (Optional but good practice): Zero out `items[size]` to clean up residual data.
6. Return the saved first element.

### Task D: Implement `getFirst()`
Easy
{: .label .label-green }

Implement `public int getFirst()`:
1. Check if the list is empty (`size == 0`). If so, throw a `java.util.NoSuchElementException`.
2. Return `items[0]`.

---

## 6. Skeleton Code: `AList.java`

Save this file as `AList.java`:

```java
import java.util.NoSuchElementException;

/**
 * An Array-Based List (AList) storing primitive integers.
 *
 * Invariants:
 * 1. The position of the next item to be inserted via addLast is always size.
 * 2. size is always the number of valid items in the AList.
 * 3. The last item in the list is always in position size - 1 (when size > 0).
 * 4. The first item in the list is always in position 0 (when size > 0).
 */
public class AList {
    private int[] items;
    private int size;

    /** Initial default capacity */
    private static final int DEFAULT_CAPACITY = 100;

    /** Creates an empty list with capacity 100. */
    public AList() {
        items = new int[DEFAULT_CAPACITY];
        size = 0;
    }

    /** Creates an empty list with specified initial capacity. */
    public AList(int capacity) {
        if (capacity <= 0) {
            throw new IllegalArgumentException("Capacity must be positive: " + capacity);
        }
        items = new int[capacity];
        size = 0;
    }

    /** Returns the number of items in the list. */
    public int size() {
        return size;
    }

    /** Returns true if the list contains no elements. */
    public boolean isEmpty() {
        return size == 0;
    }

    /**
     * Gets the ith item in the list (0 is the front).
     * Throws IndexOutOfBoundsException if index is invalid.
     */
    public int get(int i) {
        if (i < 0 || i >= size) {
            throw new IndexOutOfBoundsException("Index " + i + " out of bounds for size " + size);
        }
        return items[i];
    }

    /**
     * Inserts x into the back of the list.
     */
    public void addLast(int x) {
        if (size == items.length) {
            throw new IllegalStateException("AList capacity reached: " + items.length);
        }
        items[size] = x;
        size += 1;
    }

    /**
     * Returns the item from the back of the list.
     */
    public int getLast() {
        if (isEmpty()) {
            throw new NoSuchElementException("Cannot call getLast on an empty list");
        }
        return items[size - 1];
    }

    /**
     * Deletes item from the back of the list and returns deleted item.
     */
    public int removeLast() {
        if (isEmpty()) {
            throw new NoSuchElementException("Cannot call removeLast on an empty list");
        }
        int x = getLast();
        items[size - 1] = 0; // optional cleanup
        size -= 1;
        return x;
    }

    // =========================================================================
    // STUDENT IMPLEMENTATION TASKS
    // =========================================================================

    /**
     * Task D: Returns the first item in the list.
     * Throws NoSuchElementException if the list is empty.
     */
    public int getFirst() {
        // TODO: Implement getFirst
        // 1. Check if empty
        // 2. Return the item at index 0
        return 0; // replace with your implementation
    }

    /**
     * Task B: Inserts x into the front of the list.
     * All existing elements must be shifted one position to the right.
     *
     * Invariant notice: The item at index 0 must become x, and size increases by 1.
     */
    public void addFirst(int x) {
        // TODO: Implement addFirst
        // 1. Check if the array is full (size == items.length)
        // 2. Loop from right to left (i = size - 1 down to 0) to shift elements right
        // 3. Place x at items[0]
        // 4. Increment size
    }

    /**
     * Task C: Deletes the item from the front of the list and returns it.
     * All subsequent elements must be shifted one position to the left.
     *
     * Throws NoSuchElementException if the list is empty.
     */
    public int removeFirst() {
        // TODO: Implement removeFirst
        // 1. Check if empty (throw NoSuchElementException)
        // 2. Save the first item (items[0])
        // 3. Loop from left to right (i = 1 to size - 1) to shift elements left
        // 4. Decrement size
        // 5. Return the saved item
        return 0; // replace with your implementation
    }

    /**
     * Returns a string representation of the list in the format [e1, e2, e3].
     */
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder("[");
        for (int i = 0; i < size; i++) {
            sb.append(items[i]);
            if (i < size - 1) {
                sb.append(", ");
            }
        }
        sb.append("]");
        return sb.toString();
    }
}
```

---

## 7. Verification: `AListTest.java`

To verify that your implementation satisfies all invariants and passes edge cases, create and run `AListTest.java`. This test driver uses pure Java without external dependencies:

```java
public class AListTest {

    private static int passed = 0;
    private static int failed = 0;

    public static void main(String[] args) {
        System.out.println("=================================================");
        System.out.println("            AList Verification Tests             ");
        System.out.println("=================================================");

        testBackOperations();
        testAddFirst();
        testRemoveFirst();
        testInterleavedOperations();
        testEdgeCases();

        System.out.println("=================================================");
        System.out.println("Summary: Passed: " + passed + " | Failed: " + failed);
        if (failed == 0) {
            System.out.println(">>> ALL TESTS PASSED SUCCESSFULLY! Great work! <<<");
        } else {
            System.out.println(">>> SOME TESTS FAILED. Review output above! <<<");
        }
        System.out.println("=================================================");
    }

    private static void assertEquals(String testName, int expected, int actual) {
        if (expected == actual) {
            System.out.println("[PASS] " + testName + " => " + actual);
            passed++;
        } else {
            System.err.println("[FAIL] " + testName + " => Expected: " + expected + ", Got: " + actual);
            failed++;
        }
    }

    private static void assertTrue(String testName, boolean condition) {
        if (condition) {
            System.out.println("[PASS] " + testName);
            passed++;
        } else {
            System.err.println("[FAIL] " + testName + " => Expected true, got false");
            failed++;
        }
    }

    private static void testBackOperations() {
        System.out.println("\n--- 1. Testing Lecture Back Operations (addLast, removeLast) ---");
        AList list = new AList();
        list.addLast(10);
        list.addLast(20);
        list.addLast(30);

        assertEquals("BackOps: size after 3 addLast", 3, list.size());
        assertEquals("BackOps: get(0)", 10, list.get(0));
        assertEquals("BackOps: get(2)", 30, list.get(2));
        assertEquals("BackOps: getLast()", 30, list.getLast());

        int removed = list.removeLast();
        assertEquals("BackOps: removeLast value", 30, removed);
        assertEquals("BackOps: size after removeLast", 2, list.size());
        assertEquals("BackOps: new getLast()", 20, list.getLast());
    }

    private static void testAddFirst() {
        System.out.println("\n--- 2. Testing addFirst and getFirst ---");
        AList list = new AList();
        list.addFirst(10);
        assertEquals("addFirst: size on single item", 1, list.size());
        assertEquals("addFirst: getFirst()", 10, list.getFirst());
        assertEquals("addFirst: get(0)", 10, list.get(0));

        list.addFirst(20);
        list.addFirst(30);
        // List should now be [30, 20, 10]
        assertEquals("addFirst: size after 3 items", 3, list.size());
        assertEquals("addFirst: getFirst()", 30, list.getFirst());
        assertEquals("addFirst: get(0)", 30, list.get(0));
        assertEquals("addFirst: get(1)", 20, list.get(1));
        assertEquals("addFirst: get(2)", 10, list.get(2));
        assertEquals("addFirst: getLast()", 10, list.getLast());
    }

    private static void testRemoveFirst() {
        System.out.println("\n--- 3. Testing removeFirst ---");
        AList list = new AList();
        list.addLast(5);
        list.addLast(15);
        list.addLast(25);
        // List is [5, 15, 25]

        int r1 = list.removeFirst();
        assertEquals("removeFirst: first removed", 5, r1);
        assertEquals("removeFirst: size after 1st removal", 2, list.size());
        assertEquals("removeFirst: new getFirst()", 15, list.getFirst());
        assertEquals("removeFirst: get(0)", 15, list.get(0));
        assertEquals("removeFirst: get(1)", 25, list.get(1));

        int r2 = list.removeFirst();
        assertEquals("removeFirst: second removed", 15, r2);
        assertEquals("removeFirst: new getFirst()", 25, list.getFirst());

        int r3 = list.removeFirst();
        assertEquals("removeFirst: third removed", 25, r3);
        assertTrue("removeFirst: list is empty", list.isEmpty());
        assertEquals("removeFirst: size is 0", 0, list.size());
    }

    private static void testInterleavedOperations() {
        System.out.println("\n--- 4. Testing Interleaved Front and Back Operations ---");
        AList list = new AList();
        // Alternating front and back adds
        list.addFirst(2);
        list.addLast(3);
        list.addFirst(1);
        list.addLast(4);
        // Expected: [1, 2, 3, 4]
        assertEquals("Interleaved: size", 4, list.size());
        assertEquals("Interleaved: index 0", 1, list.get(0));
        assertEquals("Interleaved: index 1", 2, list.get(1));
        assertEquals("Interleaved: index 2", 3, list.get(2));
        assertEquals("Interleaved: index 3", 4, list.get(3));

        // Mixed removals
        assertEquals("Interleaved: removeFirst", 1, list.removeFirst()); // remaining [2, 3, 4]
        assertEquals("Interleaved: removeLast", 4, list.removeLast());   // remaining [2, 3]
        assertEquals("Interleaved: getFirst", 2, list.getFirst());
        assertEquals("Interleaved: getLast", 3, list.getLast());
        assertEquals("Interleaved: size", 2, list.size());
    }

    private static void testEdgeCases() {
        System.out.println("\n--- 5. Testing Edge Cases & Exceptions ---");
        AList list = new AList(5); // small capacity

        // Test NoSuchElementException on empty list
        boolean threwOnRemoveFirst = false;
        try {
            list.removeFirst();
        } catch (java.util.NoSuchElementException e) {
            threwOnRemoveFirst = true;
        }
        assertTrue("EdgeCase: removeFirst on empty throws NoSuchElementException", threwOnRemoveFirst);

        boolean threwOnGetFirst = false;
        try {
            list.getFirst();
        } catch (java.util.NoSuchElementException e) {
            threwOnGetFirst = true;
        }
        assertTrue("EdgeCase: getFirst on empty throws NoSuchElementException", threwOnGetFirst);

        // Fill to capacity
        for (int i = 0; i < 5; i++) {
            list.addLast(i * 10);
        }
        assertEquals("EdgeCase: list full size", 5, list.size());

        // Test capacity limit
        boolean threwOnAddFirstFull = false;
        try {
            list.addFirst(999);
        } catch (IllegalStateException e) {
            threwOnAddFirstFull = true;
        }
        assertTrue("EdgeCase: addFirst when full throws IllegalStateException", threwOnAddFirstFull);
    }
}
```

---

## 8. Discussion Questions

To demonstrate mastery, answer the following conceptual questions in your lab report:

1. **Why is `addFirst` an $$O(N)$$ operation for `AList`, whereas it is $$O(1)$$ for `DLList`?**
   * *Hint:* Think about how elements are stored in memory. Can you insert an element into index `0` of an array without moving the others?

2. **Why MUST the shifting loop in `addFirst` run from right to left (`size - 1` down to `0`) instead of left to right (`0` to `size - 1`)?**
   * *Hint:* Trace what happens to `items[1]` when you execute `items[1] = items[0]`.

3. **In `removeLast()`, we only decremented `size` without zeroing out the deleted index. Why does `removeFirst()` require moving every remaining element instead of just doing `size--`?**
   * *Hint:* What is Invariant #4 regarding index `0`?

4. **Under what application scenarios would you choose an `AList` over a `DLList`? Under what scenarios would you choose a `DLList`?**
   * *Hint:* Compare random `get(i)` frequency versus frequent insertions at the front of large collections.
