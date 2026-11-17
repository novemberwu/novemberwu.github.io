---
title: Lab 14 Merge Two Sorted Lists
nav_order: 10
parent: 2026 DSA
layout: page
math: mathjax
---

# Lab 14: Merge Two Sorted Lists (Guided Lab)
{: .no_toc}

## Goals:
{: .no_toc}
* Understand the **Two-Pointer** technique for merging two sorted linear sequences into a single sorted sequence.
* Master the **Dummy Head (Sentinel Node)** pattern to eliminate edge cases when constructing or rewiring linked lists.
* Learn how to rewire existing `ListNode` pointers **in-place**, achieving $$O(1)$$ auxiliary memory without allocating new data nodes.
* Correctly handle termination and the remaining tail when one list is exhausted before the other.
* Lay the foundation for **Divide and Conquer Sorting**: the `mergeList` algorithm implemented in this lab serves as the core merging subroutine for **Merge Sort on Linked Lists** in Lab 15.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## 1. Problem Description

[LeetCode 21 - Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/){: .btn .btn-blue target="_blank" }

You are given the heads of two sorted singly linked lists: `l1` and `l2`.

Merge the two lists into a **single sorted list**. The list should be formed by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

---

### Example 1:

```
List 1:     (1) --------> (3) --------> (5) --------> (7) --------> (9)
             ▲             ▲             ▲             ▲             ▲
             │             │             │             │             │
Merged:     (1) -> (2) -> (3) -> (4) -> (5) -> (6) -> (7) -> (8) -> (9) -> (10)
                    │             │             │             │              │
                    ▼             ▼             ▼             ▼              ▼
List 2:            (2) --------> (4) --------> (6) --------> (8) --------> (10)
```

**Input:**
```java
ListNode l1 = [1, 3, 5, 7, 9];
ListNode l2 = [2, 4, 6, 8, 10];
```
**Output:**
```java
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

---

### Example 2: Lists of Unequal Lengths

```
List 1:     (1) --------> (2) --------> (4)
             ▲             ▲             ▲
             │             │             │
Merged:     (1) -> (1) -> (2) -> (3) -> (4) -> (5) -> (6)
                    ▲                    ▲      ▲      ▲
                    │                    │      │      │
List 2:            (1) --------> (3) ---/      (5) -> (6)
```

**Input:**
```java
ListNode l1 = [1, 2, 4];
ListNode l2 = [1, 3, 4, 5, 6];
```
**Output:**
```java
[1, 1, 2, 3, 4, 4, 5, 6]
```

---

### Example 3: Empty List Edge Cases

* **Input:** `l1 = []`, `l2 = [0]`  
  **Output:** `[0]`
* **Input:** `l1 = []`, `l2 = []`  
  **Output:** `[]`

---

### Constraints:
* The number of nodes in both lists is in the range $$[0, 50]$$.
* $$-100 \le \text{Node.val} \le 100$$
* Both `l1` and `l2` are sorted in **non-decreasing** (ascending) order.

---

## 2. Conceptual Walkthrough & Visual Guide

### The Naive Mistake: Creating Brand New Nodes
When first encountering this problem, beginners often instantiate a brand new `new ListNode(val)` for every single number. While that works, it wastes memory:
* It allocates $$O(N + M)$$ additional heap objects.
* It discards the existing nodes of `l1` and `l2`, triggering garbage collection.

In data structures, **in-place pointer splicing** is preferred: we simply change the `.next` references of existing nodes so they chain together in sorted order.

---

### Key Pattern: The Dummy Head (Sentinel Node)

When constructing a linked list from scratch, determining the very first node (`head`) is usually tricky:
* Which list supplies the first node? `l1` or `l2`?
* If you don't use a dummy node, you must write special `if (head == null)` statements to assign `head`, and then treat all subsequent insertions differently.

To avoid redundant conditional checks, we use a **Dummy Node (Sentinel)**:

```
[head / dummy] -> null
      ▲
      │
      k (tail pointer)
```

1. Create a dummy node: `ListNode k = new ListNode();` and anchor `ListNode head = k;`.
2. As we choose smaller nodes from `l1` or `l2`, we simply attach them to `k.next`:
   ```java
   k.next = chosenNode;
   k = k.next; // advance the tail pointer
   ```
3. When finished, the real merged list begins at `head.next`:

```
[dummy] -> (1) -> (2) -> (3) -> (4) -> ...
            ▲
            │
       head.next  <--- Return this!
```

---

### Step-by-Step Pointer Tracing

Let's trace how three pointers (`i`, `j`, and `k`) cooperate to weave two lists:

* `i` traverses `l1`
* `j` traverses `l2`
* `k` trails behind the end of the newly merged chain

#### Initial State:
```
l1:   (1) -> (3) -> (5)
       ▲
       │
       i

l2:   (2) -> (4) -> (6)
       ▲
       │
       j

Merged:  [dummy] -> null
           ▲
           │
           k (head)
```

#### Step 1: Compare `i.val (1)` vs `j.val (2)`
* `1 < 2`: Node `1` is smaller.
* Connect `k.next = i;`
* Advance `i = i.next;`
* Advance `k = k.next;`

```
l1:          (3) -> (5)
              ▲
              │
              i

l2:   (2) -> (4) -> (6)
       ▲
       │
       j

Merged:  [dummy] -> (1)
                     ▲
                     │
                     k
```

#### Step 2: Compare `i.val (3)` vs `j.val (2)`
* `3 > 2`: Node `2` is smaller.
* Connect `k.next = j;`
* Advance `j = j.next;`
* Advance `k = k.next;`

```
l1:          (3) -> (5)
              ▲
              │
              i

l2:                 (4) -> (6)
                     ▲
                     │
                     j

Merged:  [dummy] -> (1) -> (2)
                            ▲
                            │
                            k
```

#### Step 3: What Happens When One List Runs Out?
Eventually, either `i` or `j` becomes `null`. 

Suppose `l1` runs out first (`i == null`), but `l2` still has `(8) -> (10) -> null`:
* In an array, we would need a loop to copy every remaining element.
* **In a Linked List**, all remaining nodes in `l2` are **already linked together in sorted order**!
* Therefore, we only need a single pointer assignment:
  ```java
  if (i != null) {
      k.next = i;
  } else {
      k.next = j;
  }
  ```
* This attaches the entire remaining chain in $$O(1)$$ constant time!

---

## 3. Implementation Task

Easy
{: .label .label-green }
Guided
{: .label .label-blue }

Complete the implementation of `mergeList` in `MergeList.java`.

### Starter Code Skeleton:

```java
public class MergeList {

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
     * Rewires existing nodes in-place without allocating new list nodes.
     *
     * @param l1 Head of the first sorted list
     * @param l2 Head of the second sorted list
     * @return Head of the merged sorted list
     */
    public static ListNode mergeList(ListNode l1, ListNode l2) {
        // TODO 1: Initialize pointer i to traverse l1, and pointer j to traverse l2.

        // TODO 2: Create a dummy (sentinel) node to anchor the merged list:
        //         ListNode k = new ListNode();
        //         ListNode head = k;

        // TODO 3: Loop while BOTH i and j are not null (i != null && j != null):
        //         a. Compare i.val and j.val.
        //         b. If i.val < j.val:
        //            - Point k.next to i.
        //            - Advance i to i.next.
        //            - Optional: isolate the attached node by clearing its next link,
        //              or let it be naturally overwritten in the next iteration.
        //            - Advance k to k.next.
        //         c. Otherwise (j.val <= i.val):
        //            - Point k.next to j.
        //            - Advance j to j.next.
        //            - Advance k to k.next.

        // TODO 4: After the loop terminates, check which list has remaining nodes.
        //         Connect k.next to the non-null pointer (either i or j).

        // TODO 5: Return the first valid node of the merged chain (head.next).
        return null;
    }

    public static void main(String[] args) {
        // Test 1: Standard alternating sorted lists
        System.out.println("--- Test 1: Standard Alternating Lists ---");
        ListNode l1 = getListFromArray(new int[]{1, 3, 5, 7, 9});
        ListNode l2 = getListFromArray(new int[]{2, 4, 6, 8, 10});
        System.out.print("List 1: ");
        print(l1);
        System.out.print("List 2: ");
        print(l2);
        ListNode merged1 = mergeList(l1, l2);
        System.out.print("Merged: ");
        print(merged1);
        // Expected: 1 2 3 4 5 6 7 8 9 10

        // Test 2: Different lengths and duplicate values
        System.out.println("\n--- Test 2: Unequal Lengths with Duplicates ---");
        ListNode l3 = getListFromArray(new int[]{1, 2, 4});
        ListNode l4 = getListFromArray(new int[]{1, 3, 4, 5, 6});
        System.out.print("List 3: ");
        print(l3);
        System.out.print("List 4: ");
        print(l4);
        ListNode merged2 = mergeList(l3, l4);
        System.out.print("Merged: ");
        print(merged2);
        // Expected: 1 1 2 3 4 4 5 6

        // Test 3: One list is empty
        System.out.println("\n--- Test 3: One Empty List ---");
        ListNode l5 = getListFromArray(new int[]{});
        ListNode l6 = getListFromArray(new int[]{0});
        System.out.print("List 5: ");
        print(l5);
        System.out.print("List 6: ");
        print(l6);
        ListNode merged3 = mergeList(l5, l6);
        System.out.print("Merged: ");
        print(merged3);
        // Expected: 0

        // Test 4: Both lists are empty
        System.out.println("\n--- Test 4: Both Empty Lists ---");
        ListNode l7 = getListFromArray(new int[]{});
        ListNode l8 = getListFromArray(new int[]{});
        ListNode merged4 = mergeList(l7, l8);
        System.out.print("Merged: ");
        print(merged4);
        // Expected: []

        // Test 5: Negative numbers
        System.out.println("\n--- Test 5: Negative Numbers ---");
        ListNode l9 = getListFromArray(new int[]{-9, -5, -2, 4});
        ListNode l10 = getListFromArray(new int[]{-7, -3, 0, 8});
        ListNode merged5 = mergeList(l9, l10);
        System.out.print("Merged: ");
        print(merged5);
        // Expected: -9 -7 -5 -3 -2 0 4 8
    }
}
```

---

### Expected Console Output:

```
--- Test 1: Standard Alternating Lists ---
List 1: 1 3 5 7 9 
List 2: 2 4 6 8 10 
Merged: 1 2 3 4 5 6 7 8 9 10 

--- Test 2: Unequal Lengths with Duplicates ---
List 3: 1 2 4 
List 4: 1 3 4 5 6 
Merged: 1 1 2 3 4 4 5 6 

--- Test 3: One Empty List ---
List 5: []
List 6: 0 
Merged: 0 

--- Test 4: Both Empty Lists ---
Merged: []

--- Test 5: Negative Numbers ---
Merged: -9 -7 -5 -3 -2 0 4 8 
```

---

## 4. Complexity Analysis

| Metric | Complexity | Explanation |
| :--- | :---: | :--- |
| **Time Complexity** | $$O(N + M)$$ | In the worst case, every node from both list 1 (length $$N$$) and list 2 (length $$M$$) is inspected and rewired exactly once. |
| **Space Complexity** | $$O(1)$$ | We do not create new list nodes. Only a constant number of pointer references (`i`, `j`, `k`, `head`) are allocated on the stack. |

---

## 5. Conceptual Checkpoint Questions

1. **Why do we return `head.next` instead of `head`?**
   * What value was stored in `head`? Why is `head` considered a "dummy" or "sentinel" node?
2. **Why don't we need a `while` loop to attach the leftover elements?**
   * Contrast this with merging two sorted arrays. Why does a linked list allow an $$O(1)$$ remainder attachment?
3. **In-Place vs. New Allocation:**
   * If the problem required the original `l1` and `l2` to remain unmodified (immutable), how would your implementation need to change? What would the space complexity become?
4. **Looking Ahead:**
   * In **Lab 15**, we will implement Merge Sort on a Linked List. Where does `mergeList` fit into the Divide and Conquer framework?
