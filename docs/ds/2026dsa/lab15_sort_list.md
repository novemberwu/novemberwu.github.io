---
title: Lab 15 Sort List (Merge Sort on Linked List)
nav_order: 11
parent: 2026 DSA
layout: page
math: mathjax
---

# Lab 15: Sort List (Merge Sort on Linked List)
{: .no_toc}

## Goals:
{: .no_toc}
* Solve **LeetCode 148: Sort List** using the **Divide and Conquer** paradigm.
* Understand why **Merge Sort** is the gold standard for sorting linked lists compared to arrays and Quick Sort.
* Master the **Fast & Slow Pointer (Tortoise and Hare)** technique to locate the midpoint of a linked list in $$O(N)$$ time.
* Learn how to cleanly **sever a linked list** into two independent sublists to prevent infinite recursive loops.
* Formulate base cases and recursive decomposition for dynamic pointer structures.
* Seamlessly integrate the **`mergeList`** routine developed in **Lab 14** as the combining step.
* Rigorously analyze time ($$O(N \log N)$$) and auxiliary space complexity.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## 1. Problem Description

[LeetCode 148 - Sort List](https://leetcode.com/problems/sort-list/?envType=problem-list-v2&envId=merge-sort){: .btn .btn-blue target="_blank" }

Given the `head` of a singly linked list, return the list after sorting it in **ascending order**.

**Follow-up Challenge:** Can you sort the linked list in $$O(n \log n)$$ time and $$O(1)$$ auxiliary memory (or $$O(\log n)$$ recursion stack space)?

---

### Example 1:

```
Unsorted:   (4) -------> (2) -------> (1) -------> (3)
             │            │            │            │
             ▼            ▼            ▼            ▼
Sorted:     (1) -------> (2) -------> (3) -------> (4)
```

**Input:** `head = [4, 2, 1, 3]`  
**Output:** `[1, 2, 3, 4]`

---

### Example 2: Negative Numbers and Odd Length

```
Unsorted:   (-1) ------> (5) ------> (3) ------> (4) ------> (0)
             │            │           │           │           │
             ▼            ▼           ▼           ▼           ▼
Sorted:     (-1) ------> (0) ------> (3) ------> (4) ------> (5)
```

**Input:** `head = [-1, 5, 3, 4, 0]`  
**Output:** `[-1, 0, 3, 4, 5]`

---

### Example 3: Edge Cases

* **Input:** `head = []`  
  **Output:** `[]`
* **Input:** `head = [10]`  
  **Output:** `[10]`

---

### Constraints:
* The number of nodes in the list is in the range $$[0, 5 \times 10^4]$$.
* $$-10^5 \le \text{Node.val} \le 10^5$$

---

## 2. Why Merge Sort for Linked Lists?

In introductory algorithms, we typically study sorting on contiguous arrays. How does a linked list change the trade-offs?

| Algorithm | Arrays | Singly Linked Lists | Why the Difference on Lists? |
| :--- | :---: | :---: | :--- |
| **Quick Sort** | $$O(N \log N)$$ avg, $$O(1)$$ space | Clunky & slow | Quick Sort relies heavily on random access (`arr[i]`) and cache-friendly swaps. Traversing backward or finding arbitrary partitions in singly linked lists is costly. |
| **Merge Sort** | $$O(N \log N)$$, **$$O(N)$$ extra array space** | **$$O(N \log N)$$, $$O(1)$$ extra space!** | On arrays, the merge step requires allocating a temporary auxiliary array to copy elements. **On linked lists, merging requires NO extra array!** We simply rewire existing node pointers in-place (as mastered in Lab 14)! |

> **Key Takeaway:** Merge Sort is inherently suited for linked lists because pointer relinking eliminates the $$O(N)$$ memory overhead required by array-based merge sort.

---

## 3. Divide and Conquer Architecture

Merge Sort divides the sorting problem into three distinct phases: **Divide**, **Conquer**, and **Combine**.

```
                           [4 -> 2 -> 1 -> 3]
                                   │
                     1. DIVIDE (Find mid & sever)
                                   ▼
                   [4 -> 2]                 [1 -> 3]
                     │                         │
            DIVIDE (Split again)      DIVIDE (Split again)
                     ▼                         ▼
               [4]       [2]             [1]       [3]
                 \       /                 \       /
              2. CONQUER: Base cases reached (length <= 1)
                 /       \                 /       \
                  COMBINE                   COMBINE
                 (Lab 14)                  (Lab 14)
                     ▼                         ▼
                  [2 -> 4]                  [1 -> 3]
                        \                     /
                         \                   /
                           3. FINAL COMBINE
                               (Lab 14)
                                  ▼
                         [1 -> 2 -> 3 -> 4]
```

---

### Phase 1: Divide (Fast & Slow Pointers)

To divide a linked list into two equal halves, we cannot simply calculate `mid = (left + right) / 2` because a linked list has no index-based random access.

Instead, we use the **Fast & Slow Pointer (Tortoise and Hare)** technique:
* `slow` advances **1 step** at a time.
* `fast` advances **2 steps** at a time.
* When `fast` reaches the end of the list, `slow` is located at the middle!

#### The Two-Node Split Trap (Infinite Recursion Warning!)
Consider a 2-node list: `[4 -> 2 -> null]`.
* If `slow` starts at `4` and `fast` starts at `4`:
  * `fast` jumps to `null` (after 2 steps).
  * `slow` advances to `2`.
  * If we split at `slow`, left is `[4 -> null]` and right is `[2 -> null]`. That works!
* But if our termination condition allows `slow` to land on `2` without properly severing `4 -> 2`, or if `slow` stays at `4` and we don't break the connection, one subproblem still has 2 nodes:
  * `sortList([4, 2])` calls `sortList([4, 2])` forever $$\rightarrow$$ **StackOverflowError**!

#### Clean Midpoint Idiom:
An elegant way to ensure `slow` stops right at the end of the left half is to initialize `fast = head.next`:

```java
ListNode slow = head;
ListNode fast = head.next;

while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}

// slow is at the end of the first half!
ListNode mid = slow.next;
slow.next = null; // SEVER THE LINK!
```

#### Visualizing the Split on `[4 -> 2 -> 1 -> 3]`:

```
Start:
  (4) ------> (2) ------> (1) ------> (3) ------> null
   ▲           ▲
   │           │
  slow        fast

Loop Iteration 1:
  slow moves 1 step  -> points to (2)
  fast moves 2 steps -> jumps over (1) to (3)

  (4) ------> (2) ------> (1) ------> (3) ------> null
               ▲                       ▲
               │                       │
              slow                    fast

Loop Iteration 2:
  fast.next is null -> Loop terminates!

Disconnecting:
  ListNode mid = slow.next; // mid points to (1)
  slow.next = null;         // cut connection between (2) and (1)

Resulting Sublists:
  Left:   (4) -> (2) -> null
  Right:  (1) -> (3) -> null
```

---

### Phase 2: Conquer (Base Case & Recursion)

What is the simplest linked list that is guaranteed to already be sorted?
* A list with **0 nodes** (`head == null`).
* A list with **1 node** (`head.next == null`).

```java
if (head == null || head.next == null) {
    return head;
}
```

If the list has 2 or more nodes, we recursively sort each half:
```java
ListNode leftSorted = sortList(head);
ListNode rightSorted = sortList(mid);
```

---

### Phase 3: Combine (Merge Subroutine)

Once both halves are sorted, how do we combine them into a single sorted list?

This is **precisely the algorithm you built in Lab 14 (`mergeList`)**!
```java
return merge(leftSorted, rightSorted);
```

---

## 4. Implementation Task

Medium
{: .label .label-yellow }

Complete the implementation of `sortList` in `SortList.java`.

### Starter Code Skeleton:

```java
public class SortList {

    /**
     * Definition for singly-linked list node.
     */
    static class ListNode {
        int val;
        ListNode next;

        ListNode() {}

        ListNode(int val) {
            this.val = val;
        }

        ListNode(int val, ListNode next) {
            this.val = val;
            this.next = next;
        }
    }

    /**
     * Helper method: Constructs a linked list from an array of integers.
     */
    private static ListNode getListFromArray(int[] a) {
        ListNode head = new ListNode();
        ListNode k = head;
        for (int x : a) {
            k.next = new ListNode(x);
            k = k.next;
        }
        return head.next;
    }

    /**
     * Helper method: Prints all node values in a linked list.
     */
    private static void print(ListNode n) {
        if (n == null) {
            System.out.println("[]");
            return;
        }
        while (n != null) {
            System.out.print(n.val + " ");
            n = n.next;
        }
        System.out.println();
    }

    /**
     * Merges two sorted linked lists into a single sorted list.
     * (Reused from Lab 14)
     */
    public static ListNode merge(ListNode l1, ListNode l2) {
        
    }

    /**
     * Sorts a singly linked list in ascending order using Merge Sort.
     *
     * @param head Head of the unsorted list
     * @return Head of the sorted list
     */
    public static ListNode sortList(ListNode head) {
        // TODO 1: Base case: If head is null or list contains only 1 node,
        //         it is already sorted. Return head.

        // TODO 2: Find the midpoint using the Fast & Slow pointer technique.
        //         Tip: Initialize slow = head and fast = head.next so that
        //         slow stops at the end of the left half.
        //         Advance slow by 1 step, fast by 2 steps while fast != null && fast.next != null.

        // TODO 3: Sever the list into two halves:
        //         - Save slow.next as the head of the right half (mid).
        //         - Set slow.next = null to disconnect the left half from the right half.

        // TODO 4: Recursively sort both halves:
        //         ListNode leftSorted = sortList(head);
        //         ListNode rightSorted = sortList(mid);

        // TODO 5: Merge the two sorted halves using merge() and return the result.
        return null;
    }

    public static void main(String[] args) {
        // Test 1: Even number of unsorted elements
        System.out.println("--- Test 1: Even Length Unsorted List ---");
        ListNode t1 = getListFromArray(new int[]{4, 2, 1, 3});
        System.out.print("Before: ");
        print(t1);
        ListNode s1 = sortList(t1);
        System.out.print("After:  ");
        print(s1);
        // Expected: 1 2 3 4

        // Test 2: Odd number of elements with negative values
        System.out.println("\n--- Test 2: Odd Length with Negatives ---");
        ListNode t2 = getListFromArray(new int[]{-1, 5, 3, 4, 0});
        System.out.print("Before: ");
        print(t2);
        ListNode s2 = sortList(t2);
        System.out.print("After:  ");
        print(s2);
        // Expected: -1 0 3 4 5

        // Test 3: Duplicates
        System.out.println("\n--- Test 3: Multiple Duplicates ---");
        ListNode t3 = getListFromArray(new int[]{3, 1, 4, 1, 5, 9, 2, 6, 5});
        System.out.print("Before: ");
        print(t3);
        ListNode s3 = sortList(t3);
        System.out.print("After:  ");
        print(s3);
        // Expected: 1 1 2 3 4 5 5 6 9

        // Test 4: Reverse sorted list
        System.out.println("\n--- Test 4: Reverse Sorted List ---");
        ListNode t4 = getListFromArray(new int[]{10, 8, 6, 4, 2, 0});
        System.out.print("Before: ");
        print(t4);
        ListNode s4 = sortList(t4);
        System.out.print("After:  ");
        print(s4);
        // Expected: 0 2 4 6 8 10

        // Test 5: Single element
        System.out.println("\n--- Test 5: Single Element ---");
        ListNode t5 = getListFromArray(new int[]{42});
        System.out.print("Before: ");
        print(t5);
        ListNode s5 = sortList(t5);
        System.out.print("After:  ");
        print(s5);
        // Expected: 42

        // Test 6: Empty list
        System.out.println("\n--- Test 6: Empty List ---");
        ListNode t6 = getListFromArray(new int[]{});
        System.out.print("Before: ");
        print(t6);
        ListNode s6 = sortList(t6);
        System.out.print("After:  ");
        print(s6);
        // Expected: []
    }
}
```

---

### Expected Console Output:

```
--- Test 1: Even Length Unsorted List ---
Before: 4 2 1 3 
After:  1 2 3 4 

--- Test 2: Odd Length with Negatives ---
Before: -1 5 3 4 0 
After:  -1 0 3 4 5 

--- Test 3: Multiple Duplicates ---
Before: 3 1 4 1 5 9 2 6 5 
After:  1 1 2 3 4 5 5 6 9 

--- Test 4: Reverse Sorted List ---
Before: 10 8 6 4 2 0 
After:  0 2 4 6 8 10 

--- Test 5: Single Element ---
Before: 42 
After:  42 

--- Test 6: Empty List ---
Before: []
After:  []
```

---

## 5. Complexity Analysis

### Time Complexity: $$O(N \log N)$$
We can express the execution time using the standard divide-and-conquer recurrence relation:

$$T(N) = 2 \cdot T\left(\frac{N}{2}\right) + O(N)$$

* **Divide:** Finding the middle takes $$O(N)$$ steps with the two-pointer scan. Severing the link takes $$O(1)$$.
* **Conquer:** We recursively solve two subproblems of size $$N/2$$.
* **Combine:** Merging two sorted lists of total length $$N$$ takes $$O(N)$$ operations.

By the **Master Theorem** (or drawing the recursion tree):
* The tree has $$\log_2 N$$ levels.
* At each level, the total merging and splitting work across all sublists sums to $$O(N)$$.
* Total Time = $$O(N \cdot \log N)$$.

---

### Space Complexity: $$O(\log N)$$
* Unlike array-based Merge Sort (which requires an extra auxiliary array of size $$O(N)$$), linked list Merge Sort performs all node relinking **in-place**.
* The only memory consumed is the **call stack frames** for recursion.
* Since the list is divided evenly in half at each step, the maximum depth of the call stack is $$\lceil \log_2 N \rceil$$.
* Therefore, the auxiliary space complexity is **$$O(\log N)$$**.

*(Note: An iterative bottom-up implementation using step sizes $$1, 2, 4, 8, \dots$$ can achieve true $$O(1)$$ auxiliary space, as it avoids the recursive call stack entirely!)*

---

## 6. Common Pitfalls & Debugging Tips

1. **StackOverflowError (Infinite Recursion):**
   * Did you remember `slow.next = null;`? If you don't break the pointer between the first and second half, the left list still contains all nodes, causing infinite recursive calls.
   * Did you initialize `fast = head.next`? If both `slow` and `fast` start at `head`, on a 2-node list `[4, 2]`, `slow` may end up at node `2`, and severing after `slow` leaves the left half with length 2!
2. **Losing the Rest of the List During Merge:**
   * Make sure you connect the remaining tail (`tail.next = (i != null) ? i : j;`) after the `while` loop finishes.
3. **NullPointerException on Edge Cases:**
   * Always verify that `head == null || head.next == null` is checked at the very top of `sortList`.
