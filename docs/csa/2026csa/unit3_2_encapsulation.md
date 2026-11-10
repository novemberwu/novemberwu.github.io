---
title: Unit 3 Exercise 2 - Encapsulation
layout: page
nav_order: 14
parent: 2026csa
math: mathjax
---

# Exercise 2: Encapsulation — Break the Wall, Then Fix It
{: .no_toc }

Companion slides: **Session 2**. Materials: `ch3_1/Fraction.java`, `ch3_1/ProductReview.java`, `ch3_1/Book.java`.
Related reading: [Lab 9 Part 2](lab9_creatingClass.html).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part A: Access private fields of `Fraction` (from the slides)
Easy
{: .label .label-green }

Create a client class `FractionProbe.java` in the same package. Try to compile:

```java
Fraction f = new Fraction(3, 4);
System.out.println(f.num);        // ???
```

1. Write down the **exact compiler error**.
2. Now try `f.reduce()` (a private method). Write down that error too.
3. Finally, call `f.getValue()` and `f.multiply(new Fraction(1, 2))` — these should compile. Why do these work but the others do not?

---

## Part B: Access private fields of `ProductReview` (from the slides)
Easy
{: .label .label-green }

Same idea, different class:

```java
ProductReview review = new ProductReview(5, "rachelwu", "keyboard");
System.out.println(review.rating);      // ???
review.text = "great";                  // ???
```

1. Predict **before compiling** which lines fail. Then compile and check yourself.
2. Compare with `ReviewClient.java` in the repo — how does it get the rating out without touching `rating` directly?

---

## Task C (FRQ-style): Write the accessors and mutators for `Book`
Medium
{: .label .label-yellow }

Implement, in `Book.java`, a *controlled* public interface. Signatures are given — do not change them.

**(a)** `public int getPages()` — returns the number of pages.

**(b)** `public boolean isBorrowed()` — returns `true` if the book is currently borrowed, `false` otherwise. *(AP convention: boolean accessors are named `isXxx`, not `getXxx`.)*

**(c)** `public void setPrice(double newPrice)` — sets the price, but **only** if `newPrice >= 0`. If the value is invalid, print `"Error: price must be non-negative."` and leave `price` unchanged.

**(d)** `public void setBorrowed(boolean status)` — sets the borrowed flag unconditionally.

### Code Skeleton
```java
public class Book {
    private int pages;
    private String title;
    private boolean borrowed;
    private double price;

    public Book(String title, int pages, double price) {
        this.title = title;
        this.pages = pages;
        this.price = price;
        this.borrowed = false;
    }

    // TODO (a)

    // TODO (b)

    // TODO (c)

    // TODO (d)
}
```

### Tester — `LibraryClient.java`
```java
public class LibraryClient {
    public static void main(String[] args) {
        Book b = new Book("Clean Code", 464, 32.50);
        System.out.println(b.getPages());       // 464
        System.out.println(b.isBorrowed());     // false
        b.setBorrowed(true);
        System.out.println(b.isBorrowed());     // true
        b.setPrice(28.00);
        System.out.println(b.getPages() + " " + b.isBorrowed());  // still works
        b.setPrice(-5);                         // Error: price must be non-negative.
        // b.price = -5;   // uncomment: rating has private access... style error
    }
}
```

**Checkpoint:** the commented-out line must produce a compiler error if uncommented. That error **is** encapsulation working.
