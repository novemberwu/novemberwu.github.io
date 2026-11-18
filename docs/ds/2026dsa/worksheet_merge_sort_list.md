---
title: Worksheet - Merge & Sort List
nav_order: 12
parent: 2026 DSA
layout: page
math: mathjax
---

# In-Class Worksheet: Merging & Sorting Linked Lists
{: .no_toc }

[Download Printable 1-Page PDF Worksheet](worksheet_merge_sort_list.pdf){: .btn .btn-blue target="_blank" }
{: .mb-4 }

**Name:** ___________________________ &nbsp;&nbsp;&nbsp;&nbsp; **Date:** _________________ &nbsp;&nbsp;&nbsp;&nbsp; **Period:** _______

**Instructions:** Closed-computer paper exercise. Complete all 5 parts. (Time: 20 minutes)

---

### Part 1: The Sentinel (Dummy Head) Pattern (Lab 14)

1. When merging two sorted lists, we initialize:
   ```java
   ListNode dummy = new ListNode();
   ListNode tail = dummy;
   ```
   **Why do we use `dummy` instead of directly picking `l1` or `l2` as the head?**  
   *(Check the best answer)*
   - [ ] To allocate memory for all incoming values ahead of time.
   - [ ] To avoid writing special conditional branching for the very first node.
   - [ ] Because Java requires all linked lists to have at least one dummy node.

2. At the end of `mergeList()`, what line of code returns the true sorted list?
   $$\text{return } \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_;$$

3. After the `while (i != null && j != null)` loop finishes, one list may have remaining nodes.  
   **Fill in the blank** to attach the leftover chain in **one line ($$O(1)$$ time)** without a loop:
   ```java
   tail.next = (________________) ? i : j;
   ```

---

### Part 2: Pointer Tracing (Lab 14)

Trace the first two iterations of `mergeList(l1, l2)`:
* **`l1`:** `(1) -> (5) -> (9)`
* **`l2`:** `(2) -> (4)`

```
Initial State:
  dummy -> null
    ▲
    tail       i points to (1),  j points to (2)
```

1. **Iteration 1:** Which node is smaller (`1` vs `2`)? &nbsp; `Node: ______`
   * `tail.next` connects to: `( ____ )`
   * Which pointer advances (`i` or `j`)? &nbsp; `______`
   * `tail` moves to: `( ____ )`

2. **Iteration 2:** Which node is smaller (`5` vs `2`)? &nbsp; `Node: ______`
   * `tail.next` connects to: `( ____ )`
   * Which pointer advances (`i` or `j`)? &nbsp; `______`
   * `tail` moves to: `( ____ )`

---

### Part 3: Midpoint & The Severing Cut (Lab 15)

To split a list into two halves for Merge Sort:
```java
ListNode slow = head;
ListNode fast = head.next; // Notice: starts 1 step ahead!

while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
```

Given the list: `(4) -> (2) -> (1) -> (3) -> null`

1. When the `while` loop finishes:
   * `slow` points to node with value: `[ ______ ]`
   * `fast` points to node with value: `[ ______ ]`

2. Write the **two lines of code** needed to disconnect the list into `left` and `right` sublists:
   ```java
   ListNode mid = ____________________________; // 1. Save right half head

   ___________________________________________; // 2. SEVER the link!
   ```

---

### Part 4: Spot the Bug! (Infinite Recursion Trap)

A student writes this helper to split a 2-node list `head = [4 -> 2 -> null]`:

```java
ListNode slow = head;
ListNode fast = head; // BUG HERE!

while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}

ListNode mid = slow.next;
slow.next = null;
```

1. Where does `slow` end up after the loop? &nbsp; Node with value: `[ ______ ]`
2. What are the contents of the two sublists after `slow.next = null`?
   * Left sublist: `[ ___________________________ ]`
   * Right sublist (`mid`): `[ ___________________________ ]`
3. What catastrophic runtime error occurs when `sortList(head)` is called with these sublists?  
   **Why?** __________________________________________________________________________________

---

### Part 5: Complexity Match & Reflection

| Operation | Time Complexity | Auxiliary Space Complexity |
| :--- | :---: | :---: |
| Merging two lists of length $$N$$ and $$M$$ | $$O(\quad\quad)$$ | $$O(\quad\quad)$$ |
| Find Midpoint (Slow/Fast Pointers) | $$O(\quad\quad)$$ | $$O(\quad\quad)$$ |
| Full `sortList` (Merge Sort on Linked List) | $$O(\quad\quad)$$ | $$O(\quad\quad)$$ |
*(Choose from: $$O(1)$$, $$O(N)$$, $$O(\log N)$$, $$O(N + M)$$, $$O(N \log N)$$, $$O(N^2)$$)*

**Quick Reflection:** Why does Merge Sort on a *Linked List* require only $$O(1)$$ auxiliary node allocation, whereas Merge Sort on an *Array* requires $$O(N)$$ extra space?  
__________________________________________________________________________________  
__________________________________________________________________________________

---

## Teacher Answer Key & Scoring Guide (20 Points Total)
{: .no_toc }

<details>
<summary><strong>Click to expand Teacher Answer Key</strong></summary>

### Part 1: Sentinel Pattern (4 pts)
1. **[X] To avoid writing special conditional branching for the very first node.** (2 pts)
2. `return dummy.next;` (or `head.next;`) (1 pt)
3. `tail.next = (i != null) ? i : j;` (1 pt)

### Part 2: Pointer Tracing (4 pts)
* **Iteration 1:** Node **1** is smaller. `tail.next` connects to **(1)**. Pointer **`i = i.next`** advances. `tail` moves to node **(1)**. (2 pts)
* **Iteration 2:** Node **2** is smaller. `tail.next` connects to **(2)**. Pointer **`j = j.next`** advances. `tail` moves to node **(2)**. (2 pts)

### Part 3: Midpoint & Severing Cut (4 pts)
1. `slow` stops at node **[ 2 ]**, `fast` stops at node **[ 3 ]**. (2 pts)
2. Lines of code: (2 pts)
   - `ListNode mid = slow.next;`
   - `slow.next = null;`

### Part 4: Spot the Bug! (4 pts)
1. `slow` ends up at node **[ 2 ]**. (1 pt)
2. Sublists:
   - Left sublist: `[ 4 -> 2 -> null ]` (size 2) (1 pt)
   - Right sublist (`mid`): `null` (size 0) (1 pt)
3. **Fatal Error:** `StackOverflowError` (Infinite Recursion). Because the left half was not reduced in size, `sortList([4, 2])` calls `sortList([4, 2])` infinitely. (1 pt)

### Part 5: Complexity & Reflection (4 pts)
* `mergeList`: Time **$$O(N + M)$$**, Space **$$O(1)$$** (1 pt)
* Find Midpoint: Time **$$O(N)$$**, Space **$$O(1)$$** (1 pt)
* `sortList`: Time **$$O(N \log N)$$**, Space **$$O(\log N)$$** (recursion stack) (1 pt)
* **Reflection:** Linked lists can rewire existing `.next` pointer links in-place without creating new objects, whereas contiguous arrays cannot insert or weave items without allocating an auxiliary buffer of size $$N$$ to prevent overwriting unmerged elements. (1 pt)

</details>

