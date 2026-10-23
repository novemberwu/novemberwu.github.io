---
title: Lab 13 Number of Provinces
nav_order: 9
parent: 2026 DSA
layout: page
math: mathjax
---

# Lab 13: Number of Provinces
{: .no_toc}

## Goals:
{: .no_toc}
* Understand the **Number of Provinces** problem ([LeetCode 547](https://leetcode.com/problems/number-of-provinces/description/?envType=problem-list-v2&envId=union-find)).
* Model connected components using the **Union-Find (Disjoint Set)** data structure.
* Implement `UnionFind` from scratch (initialization, `find`, `union`, and `count`).
* Implement `findCircleNum(int[][] isConnected)` to count the total number of connected provinces.
* Verify the implementation against example inputs and edge cases.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## 1. Problem Description

[LeetCode 547 - Number of Provinces](https://leetcode.com/problems/number-of-provinces/description/?envType=problem-list-v2&envId=union-find){: .btn .btn-blue target="_blank" }

There are $$n$$ cities numbered from $$0$$ to $$n - 1$$. Some of them are connected directly by roads, while some are not.

* If city $$a$$ is connected directly with city $$b$$, and city $$b$$ is connected directly with city $$c$$, then city $$a$$ is connected **indirectly** with city $$c$$.
* A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an $$n \times n$$ matrix `isConnected` where:
* `isConnected[i][j] = 1` if the $$i$$-th city and the $$j$$-th city are directly connected.
* `isConnected[i][j] = 0` otherwise.

**Goal:** Return the total number of **provinces**.

---

### Example 1:

```
        (0) <=======> (1)              (2)
        
         City 0 and City 1           City 2
         are connected.              is isolated.
         [ Province 1 ]              [ Province 2 ]
```

**Input:**
```java
int[][] isConnected = {
    {1, 1, 0},
    {1, 1, 0},
    {0, 0, 1}
};
```
**Output:** `2`  
**Explanation:** City `0` and City `1` are directly connected, forming one province. City `2` is isolated, forming its own province. Total = `2`.

---

### Example 2:

```
        (0)             (1)             (2)
      City 0          City 1          City 2
   [ Province 1 ]  [ Province 2 ]  [ Province 3 ]
```

**Input:**
```java
int[][] isConnected = {
    {1, 0, 0},
    {0, 1, 0},
    {0, 0, 1}
};
```
**Output:** `3`  
**Explanation:** No cities are connected to each other. Each city forms its own province. Total = `3`.

---

### Example 3: Transitive Connectivity

```
        (0) <-------> (1) <-------> (2)          (3)
        
            Cities 0, 1, and 2                  City 3
            are connected transitively.         is isolated.
            [ Province 1 ]                      [ Province 2 ]
```

**Input:**
```java
int[][] isConnected = {
    {1, 1, 0, 0},
    {1, 1, 1, 0},
    {0, 1, 1, 0},
    {0, 0, 0, 1}
};
```
**Output:** `2`  
**Explanation:** City `0` connects to City `1`, and City `1` connects to City `2`. Therefore, Cities `0`, `1`, and `2` all belong to the same province. City `3` is alone. Total = `2`.

---

### Constraints:
* $$1 \le n \le 200$$
* `n == isConnected.length`
* `n == isConnected[i].length`
* `isConnected[i][j]` is `1` or `0`
* `isConnected[i][i] == 1` (a city is always connected to itself)
* `isConnected[i][j] == isConnected[j][i]` (roads are bidirectional / matrix is symmetric)

---

## 2. Union-Find Modeling

* **Elements:** Each city from $$0$$ to $$n - 1$$ represents an element in the disjoint set.
* **Connectivity:** A road between city $$i$$ and city $$j$$ (`isConnected[i][j] == 1`) corresponds to a `union(i, j)` operation.
* **Provinces:** The total number of provinces is equal to the number of remaining disjoint connected components after processing all roads.

---

## 3. Implementation Task

Medium
{: .label .label-yellow }

Complete the implementation of `NumberOfProvinces.java`.

### Code Skeleton:

```java
public class NumberOfProvinces {

    /**
     * Union-Find (Disjoint Set) data structure.
     */
    static class UnionFind {
        // TODO 1: Declare instance variables (e.g., parent, size or rank, count)

        public UnionFind(int n) {
            // TODO 2: Initialize data structure with n isolated elements
        }

        /**
         * Returns the canonical root of element p.
         */
        public int find(int p) {
            // TODO 3: Implement find (recommend path compression)
            return -1;
        }

        /**
         * Merges the sets containing element p and element q.
         */
        public void union(int p, int q) {
            // TODO 4: Merge sets containing p and q, and update component count
        }

        /**
         * Returns the number of disjoint components.
         */
        public int count() {
            // TODO 5: Return the number of disjoint sets
            return 0;
        }
    }

    /**
     * Returns the total number of provinces.
     */
    public static int findCircleNum(int[][] isConnected) {
        // TODO 6: Complete findCircleNum using UnionFind
        return 0;
    }

    public static void main(String[] args) {
        // Test Case 1: Example 1 (2 provinces)
        int[][] test1 = {
            {1, 1, 0},
            {1, 1, 0},
            {0, 0, 1}
        };
        System.out.println("Test 1 Provinces: " + findCircleNum(test1)); // Expected: 2

        // Test Case 2: Example 2 (3 isolated cities)
        int[][] test2 = {
            {1, 0, 0},
            {0, 1, 0},
            {0, 0, 1}
        };
        System.out.println("Test 2 Provinces: " + findCircleNum(test2)); // Expected: 3

        // Test Case 3: Transitive connectivity (2 provinces)
        int[][] test3 = {
            {1, 1, 0, 0},
            {1, 1, 1, 0},
            {0, 1, 1, 0},
            {0, 0, 0, 1}
        };
        System.out.println("Test 3 Provinces: " + findCircleNum(test3)); // Expected: 2

        // Test Case 4: All cities connected in a line (1 province)
        int[][] test4 = {
            {1, 1, 0, 0},
            {1, 1, 1, 0},
            {0, 1, 1, 1},
            {0, 0, 1, 1}
        };
        System.out.println("Test 4 Provinces: " + findCircleNum(test4)); // Expected: 1

        // Test Case 5: Single city (1 province)
        int[][] test5 = {
            {1}
        };
        System.out.println("Test 5 Provinces: " + findCircleNum(test5)); // Expected: 1
    }
}
```

### Expected Output:
```
Test 1 Provinces: 2
Test 2 Provinces: 3
Test 3 Provinces: 2
Test 4 Provinces: 1
Test 5 Provinces: 1
```
