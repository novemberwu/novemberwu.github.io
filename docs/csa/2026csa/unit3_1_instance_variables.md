---
title: Unit 3 Exercise 1 - Instance Variables
layout: page
nav_order: 13
parent: 2026csa
math: mathjax
---

# Exercise 1: Instance Variables — Give an Object Its State
{: .no_toc }

Companion slides: **Session 1** (`unit3_class_slides.html`). Materials: `ch3_1/Book.java`, `ch3_1/ProductReview.java`.

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

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Warm-up MC (5 min)

Which declaration correctly declares an immutable instance variable holding a product's ID?

* (A) `public int productId;`
* (B) `private final int productId;`
* (C) `private int final productId;`
* (D) `final private productId int;`

<details markdown="block">
<summary>Answer</summary>
(B). Instance variables are usually <code>private</code>; <code>final</code> comes before the type: <code>private final int productId;</code>
</details>

---

## Task A: Which fields are immutable?
Medium
{: .label .label-yellow }

`Book.java` (repo `ch3_1`) currently contains:

```java
public class Book {
    private int pages;
    private String title;
    private boolean borrowed;
    private double price;
}
```

1. Decide, for each field, whether a real library book's record should be **immutable** (add `final`) or **mutable**.
2. Justify each choice in one sentence. (Example reasoning: a book's page count never changes after printing, but whether it is borrowed changes with every checkout.)
3. Add the `final` keyword where appropriate.

**Hint (from the slides):** `final` freezes the *reference*. A `final` `String` can never point to another `String` — but the object it points to may still have its own behavior. For AP CSA purposes: primitives and `String` that should never change → `final`.

---

## Task B: Constructors for `Book`
Medium
{: .label .label-yellow }

`Book` has no constructor yet. Add:

1. A **no-args constructor** with sensible defaults (`pages = 0`, `title = "untitled"`, `borrowed = false`, `price = 0.0`).
2. A **full constructor** taking `(String title, int pages, double price)` — `borrowed` always starts as `false`.

### Code Skeleton
```java
public class Book {
    // TODO 1: add final where appropriate (Task A)

    // TODO 2: no-args constructor

    // TODO 3: full constructor
    public Book(String title, int pages, double price) {
        // Your code here
    }
}
```

### Expected behavior
```java
Book b1 = new Book();
// b1.title is "untitled", b1.borrowed is false
Book b2 = new Book("Effective Java", 412, 45.99);
// b2.pages is 412, b2.borrowed is false
```

---

## Task C (FRQ-style): Spot the illegal declaration
Which of the following will **not** compile? Explain each.

```java
public class Quiz {
    private int a = 3;               // 1
    private final int b;             // 2
    public String private c;         // 3
    private int d, e;                // 4
    private void f = 2.5;            // 5
}
```

<details markdown="block">
<summary>Answer</summary>
3 — <code>public</code> and <code>private</code> cannot both appear (and the syntax <code>String private c</code> is invalid). 5 — a variable cannot be declared with two types (<code>void</code> is not a variable type). 1, 2, 4 are legal.
</details>
