---
title: Unit 3 Exercise 6 - Integration FRQ
layout: page
nav_order: 18
parent: 2026csa
math: mathjax
---

# Exercise 6: Integration FRQ — One Complete Class, 15 Minutes
{: .no_toc }

Companion slides: **Session 6**. This is the final exercise of Unit 3: everything from sessions 1–5 appears exactly once.

**Timing:** 15 minutes, closed computer. Then 5 minutes: score a classmate with the rubric below.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## The FRQ

A market research company records product reviews. The partial class `ReviewLog` below represents the log of reviews for **one** product. You will write parts of the class.

```java
public class ReviewLog {
    /** The product being reviewed; never changes after construction. */
    private final String productId;

    /** The ratings collected so far; entry k is the k-th review's rating (1-5). */
    private int[] ratings;

    /** The number of ratings stored in the array. */
    private int count;

    public ReviewLog(String productId, int capacity) { /* to be written in part (a) */ }

    /** Returns true if r is between 1 and 5 (inclusive), false otherwise. */
    public boolean isValidRating(int r) { /* implementation not shown */ }

    /** Returns the average of all stored ratings, or -1 if none exist. */
    public double averageRating() { /* to be written in part (b) */ }

    /** Records r as a new rating if it is valid and the array has room;
     *  returns true in that case, and false otherwise. */
    public boolean recordRating(int r) { /* to be written in part (c) */ }

    // Other fields and methods are not shown.
}
```

### (a) The constructor — 3 points
Write the constructor. It must:
* validate `productId` is not `null` — if it is, throw an `IllegalArgumentException`;
* validate `capacity >= 1` — if not, throw an `IllegalArgumentException`;
* initialize all instance variables.

*(Tests sessions 1 & 3: field declarations, `final`, constructors with preconditions and exceptions.)*

### (b) `averageRating` — 4 points
Returns the average of the stored ratings as a `double`. If no ratings exist, return `-1`. A full-credit solution returns the Boolean-free numeric expression in a single `return` at the end; intermediate `return` statements are only allowed inside conditionals. *(Tests session 4: methods, `return` patterns, arrays with a `count` field rather than `ratings.length`.)*

### (c) `recordRating` — 3 points
Stores `r` if **both** conditions hold: `r` is valid (reuse `isValidRating` — do not duplicate the range check) and `count < ratings.length`. In that case append `r` at index `count`, increment `count`, and return `true`. Otherwise return `false` and leave the object unchanged. *(Tests session 2 & 4: encapsulation, early `return` in a boolean/void context, reusing helper methods.)*

### (d) Verbal question — for your partner to ask you
> "Why does `averageRating` use `count` instead of `ratings.length`?"

Answer out loud in one sentence. *(Tests session 5: state vs. capacity — the tail of the array is not meaningful data.)*

---

## Scoring guide (peer review, 10 points total)

| Item | Points |
|---|---|
| (a) throws `IllegalArgumentException` with a message for both invalid inputs | 1 + 1 |
| (a) all three fields initialized, `ratings` sized `capacity` | 1 |
| (b) returns `-1` when `count == 0` | 1 |
| (b) sum loop over exactly `count` entries; division casts/produces a `double` (e.g. `(double) sum / count`) | 2 |
| (c) reuses `isValidRating` (no duplicated range check) | 1 |
| (c) appends at `count` **then** increments `count`; returns `true`/`false` correctly | 1 |
| (d) correct one-sentence answer | 1 |
| Style: no `public` fields, helper reuse, correct `final` usage | 1 |

---

## After the FRQ (on computer)

Add to `ReviewLog`:

1. `public int bestRating()` — returns the highest stored rating, or `-1` if none. *(One pass, no sorting.)*
2. A **static** factory: `public static ReviewLog defaultLog()` — returns a `new ReviewLog("default", 10)`.
3. In the tester, try `ReviewLog.averageRating();` and read the compiler error. Explain it using the Session 6 "Trying and thinking" answer.
