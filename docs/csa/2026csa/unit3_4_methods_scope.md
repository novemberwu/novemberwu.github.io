---
title: Unit 3 Exercise 4 - Methods, return & Scope
layout: page
nav_order: 16
parent: 2026csa
math: mathjax
---

# Exercise 4: Methods, `return` & Scope
{: .no_toc }

Companion slides: **Session 4**. Materials: `ch3_1/Stopwatch.java`, `ch3_1/StopwatchClient.java`, `ch3_1/ProductReview.java`.
Related reading: [Lab 9 Part 2](lab9_creatingClass.html).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part A: Debug `Stopwatch`
Medium
{: .label .label-yellow }

`ch3_1/Stopwatch.java`:

```java
public class Stopwatch {
    private final long startTime;

    public Stopwatch() {
        startTime = System.currentTimeMillis();
    }

    public double elapsedTime() {
        long endTime = System.currentTimeMillis();
        return endTime - startTime;
    }
}
```

1. The method is declared `double` — but what *unit* does it actually return?
2. `StopwatchClient.java` treats the result as **seconds**. Fix `elapsedTime()` so it truly returns elapsed seconds as a `double` (keep the declaration `public double elapsedTime()`).
3. **Type rule check:** `endTime - startTime` is a `long`. What conversion happens on `return`, and why is it allowed? *(Hint: smaller type promoted to larger type — Session 5 will formalize this.)*

### Expected behavior
```java
Stopwatch sw = new Stopwatch();
// ... do some work, e.g. a busy loop ...
System.out.println(sw.elapsedTime());   // something like 2.137 (seconds), not 2137
```

---

## Part B (FRQ-style): Validate the rating
Medium
{: .label .label-yellow }

From the original slide: *"Adding method to ProductReview to validate the rating field — if rating is between 1 and 5 (inclusive), it's valid; otherwise it's invalid."*

**(a)** Write `public boolean isValidRating()` for `ProductReview`. Return the Boolean expression **directly** — do not write `if (...) return true; else return false;`.

**(b)** Write `public boolean isPositive()` — returns `true` if `rating >= 4`.

**(c)** Write `public void setRating(int newRating)` — wait: `rating` is `final`! Explain why this mutator is **impossible** for the current `ProductReview`, and what you would have to change in the field declaration to make it possible.

### Code Skeleton
```java
public class ProductReview {
    private final int rating;
    // ... other fields ...

    // TODO (a)
    public boolean isValidRating() {
        // Your code here
    }

    // TODO (b)
    public boolean isPositive() {
        // Your code here
    }
}
```

---

## Part C: Fix the `Info` class (from the MC slide)
Medium
{: .label .label-yellow }

This class does not compile:

```java
public class Info {
    private String name;
    private int number;

    public Info(String n, int num) {
        name = n;
        number = num;
    }

    public void changeName(String newName) {
        name = newName;
    }

    public int addNum(int n) {
        num += n;
        return num;
    }
}
```

1. Explain the compile error (one sentence).
2. Fix it with a **one-word change**.
3. Bonus: the fixed `addNum` still has a design smell — the parameter `n` and the field it updates could be confused. Rewrite `addNum` using `this` to make it unambiguous.

---

## Part D: Fix the `Circle` class (from the fix-it slide)
Hard
{: .label .label-red }

```java
public class Circle {
    private double radius;

    public double computeArea() {
        private double pi = 3.14159;
        public double area = pi * radius * radius;
        return area;
    }

    // Constructor not shown.
}
```

There are **three distinct problems** in `computeArea()`. Find all three, quoting the scope rule each one violates:

1. A modifier that local variables are not allowed to have (which line(s)?).
2. A second occurrence of the same rule-breaking modifier.
3. After removing the illegal modifiers, one line still needs `this.` added to compile cleanly against the constructor `public Circle(double r) { this.radius = r; }` — why is it already fine without it, and when would it *not* be fine?

<details markdown="block">
<summary>Answer outline</summary>
Lines 1–2 of the method body: <code>private</code> and <code>public</code> are illegal on local variables ("Local variables declared inside a method cannot be declared as public or private"). After cleanup, <code>area = pi * radius * radius</code> compiles because <code>radius</code> has no same-named local shadow; it would break if someone later declared a local <code>radius</code> in the method — then the field would be shadowed.
</details>
