---
title: Unit 3 Exercise 3 - Constructors
layout: page
nav_order: 15
parent: 2026csa
math: mathjax
---

# Exercise 3: Constructors — More Constructors for `ProductReview`
{: .no_toc }

Companion slides: **Session 3**. Model to imitate: `ch3_1/Fraction.java` (four constructors, validation, copy constructor).
Original slide prompt: *"Referencing Fraction Class constructors, add more constructors to ProductReview. Validate the arguments whenever needed."*

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Warm-up trace (5 min)
Given `Fraction.java`, what does each line produce or throw?

```java
Fraction a = new Fraction();          // 1
Fraction b = new Fraction(5);         // 2
Fraction c = new Fraction(6, 0);      // 3
Fraction d = new Fraction(c);         // 4  (assume line 3 was commented out)
Fraction e = new Fraction(4, 6);      // 5
```

<details markdown="block">
<summary>Answer</summary>
1: 0/1 &nbsp; 2: 5/1 &nbsp; 3: throws <code>IllegalArgumentException</code> (denominator is 0) &nbsp; 4: 0/1 (copy of the no-args fraction) &nbsp; 5: 2/3 (reduced by <code>reduce()</code>)
</details>

---

## FRQ — `ProductReview` constructors
Medium
{: .label .label-yellow }

The class starts like this (repo `ch3_1/ProductReview.java`):

```java
public class ProductReview {
    private final int rating;
    private String text;
    private final String authorId;
    private final String productId;
    private String[] images;

    public ProductReview(int rating, String authorId, String productId) {
        this.rating = rating;
        this.authorId = authorId;
        this.productId = productId;
    }

    public ProductReview() {
        this.rating = 5;
        this.authorId = "anonymous";
        this.productId = "default value";
    }
    // getters not shown
}
```

**(a)** The 3-argument constructor must **validate** `rating`. If `rating` is not between 1 and 5 (inclusive), throw an `IllegalArgumentException` with a descriptive message. Otherwise initialize the fields and set `text` to `""`. *(Imitate `Fraction(int n, int d)`.)*

**(b)** Rewrite the **no-args constructor** to use `this(...)`, so it does not duplicate field assignments. It should create a review equivalent to `new ProductReview(5, "anonymous", "default value")`.

**(c)** Add a **copy constructor** `public ProductReview(ProductReview other)` that copies every field from `other`. Note that `rating`, `authorId`, `productId` are `final` — explain in a comment why the copy constructor is still allowed to assign them.

**(d) Trace:** for each line, state the result or the exception:

```java
ProductReview r1 = new ProductReview(7, "amy", "mouse");   // 1
ProductReview r2 = new ProductReview(3, "amy", "mouse");   // 2
ProductReview r3 = new ProductReview(r2);                  // 3
ProductReview r4 = new ProductReview();                    // 4
```

<details markdown="block">
<summary>Answer (d)</summary>
1: throws <code>IllegalArgumentException</code> &nbsp; 2: fields initialized, text = "" &nbsp; 3: copy of r2 &nbsp; 4: rating 5, author "anonymous", product "default value"
</details>

---

## Going further
Hard
{: .label .label-red }

Rewrite **all** constructors so that they chain through `this(...)` in a single "master" constructor that owns the validation. No constructor may assign fields directly. (This is exactly the pattern in slide *"Constructors can call each other with `this`"*.)
