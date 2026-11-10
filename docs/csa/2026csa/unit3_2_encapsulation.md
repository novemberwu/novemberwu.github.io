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

## Reference: `ch3_1/ProductReview.java`

<details markdown="block">
<summary>Full source, exactly as in the repo — click to expand</summary>

```java
package ch3_1;

public class ProductReview {
    private final int rating;
    private String text;
    private final String authorId;
    private final String productId;
    private  String[] images;

    public int getRating(){
        return this.rating;
    }

    public void setText(String t){
        this.text = t;

    }

    public String getText(){
        return this.text;
    }

    private final static int IMAGE_SIZE = 10;

    public ProductReview(int rating, String authorId, String pid){
        this.rating = rating;
        this.authorId = authorId;
        this.productId = pid;

    }

    public ProductReview(){
        this.rating = 5;
        this.authorId = "anonumous";
        this.productId = "default value";
    }


}
```

</details>

## Reference: `ch3_1/Book.java`

<details markdown="block">
<summary>Full source, exactly as in the repo — click to expand</summary>

```java
package ch3_1;

public class Book {
    private int pages;
    private String title;
    private boolean borrowed;
    private double price;
}
```

</details>

## Reference: `ch3_1/Fraction.java`

<details markdown="block">
<summary>Full source, exactly as in the repo — click to expand</summary>

```java
package ch3_1;

/**
 * Represents a fraction with an int numerator and int denominator
 * and provides methods for adding and multiplying fractions.
 *
 * Author: Alex
 */

public class Fraction
{

    // ******************  Instance variables  ******************

    private int num;
    private int denom;

    // *********************  Constructors  *********************

    public Fraction()       // no-args constructor
    {
        num = 0;
        denom = 1;
    }

    public Fraction(int n)
    {
        num = n;
        denom = 1;
    }

    public Fraction(int n, int d)
    {
        if (d != 0)
        {
            num = n;
            denom = d;
            reduce();
        }
        else
        {
            throw new IllegalArgumentException(
                    "Fraction construction error: denominator is 0");
        }
    }

    public Fraction(Fraction other)  // copy constructor
    {
        num = other.num;
        denom = other.denom;
    }

    // ********************  Public methods  ********************

    // Returns the sum of this fraction and other
    public Fraction add(Fraction other)
    {
        int newNum = num * other.denom + denom * other.num;
        int newDenom = denom * other.denom;
        return new Fraction(newNum, newDenom);
    }

    // Returns the sum of this fraction and m
    public Fraction add(int m)
    {
        return new Fraction(num + m * denom, denom);
    }

    // Returns the product of this fraction and other
    public Fraction multiply(Fraction other)
    {
        int newNum = num * other.num;
        int newDenom = denom * other.denom;
        return new Fraction(newNum, newDenom);
    }

    // Returns the product of this fraction and m
    public Fraction multiply(int m)
    {
        return new Fraction(num * m, denom);
    }

    // Returns the value of this fraction as a double
    public double getValue()
    {
        return (double)num / (double)denom;
    }

    // Returns a string representation of this fraction
    public String toString()
    {
        return num + "/" + denom;
    }

    // *******************  Private methods  ********************

    // Reduces this fraction by the gcf and makes denom > 0
    private void reduce()
    {
        if (num == 0)
        {
            denom = 1;
            return;
        }

        if (denom < 0)
        {
            num = -num;
            denom = -denom;
        }

        int q = gcf(Math.abs(num), denom);
        num /= q;
        denom /= q;
    }

    // Returns the greatest common factor of two positive integers
    private int gcf(int n, int d)
    {
        if (n <= 0 || d <= 0)
        {
            throw new IllegalArgumentException(
                    "gcf precondition failed: " + n + ", " + d);
        }

        while (d != 0)
        {
            int temp = d;
            d = n % d;
            n = temp;
        }
        return n;
    }
}
```

</details>

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
