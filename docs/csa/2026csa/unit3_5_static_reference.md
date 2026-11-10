---
title: Unit 3 Exercise 5 - Static & Reference
layout: page
nav_order: 17
parent: 2026csa
math: mathjax
---

# Exercise 5: Static & Parameter Passing
{: .no_toc }

Companion slides: **Session 5**. Materials: `ch3_1/MugsExamples.java`, `ch3_1/ProductReview.java`.
Related reading: [lab12_static.md](../lab12_static.html), [lab13_passbyreference.md](../lab13_passbyreference.html).

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

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part A (FRQ trace): `MugsExamples` output
Medium
{: .label .label-yellow }

From `ch3_1/MugsExamples.java`:

```java
public static void spill(Mug myMug) {
    myMug.setContents("nothing");
}

public static void reassignMug(Mug myMug) {
    myMug = new Mug("new contents");
    System.out.println("Inside reassignMug method: " + myMug.getContents());
}

public static void main(String[] args) {
    Mug teaMug = new Mug("tea");
    System.out.println("Before spill method: " + teaMug.getContents());
    spill(teaMug);
    System.out.println("After spill method: " + teaMug.getContents());
    System.out.println("\nBefore reassignMug method: " + teaMug.getContents());
    reassignMug(teaMug);
    System.out.println("After reassignMug method: " + teaMug.getContents());
}
```

**(a)** Write the exact output, **no computer allowed**.

**(b)** Draw the box-and-pointer diagram after each of the three labeled points:
1. just before `spill` returns,
2. inside `reassignMug` right after the `new Mug(...)` line executes,
3. after `main` finishes.

**(c)** In one sentence each, explain:
* why `spill` **does** change `teaMug`,
* why `reassignMug` **does not** change `teaMug`.

<details markdown="block">
<summary>Answer (a)</summary>
<pre>Before spill method: tea
After spill method: nothing

Before reassignMug method: nothing
Inside reassignMug method: new contents
After reassignMug method: nothing</pre>
The reference is copied, not the object. <code>spill</code> follows the copied reference to the <i>same</i> object and mutates it; <code>reassignMug</code> only points its <i>local copy</i> of the reference at a new object — the caller's variable is untouched.
</details>

---

## Part B: Add a static counter to `ProductReview`
Medium
{: .label .label-yellow }

**(a)** Add `private static int reviewCount = 0;` to `ProductReview` and increment it in **every** constructor. (Tip: if you completed Exercise 3, increment only in the master constructor.)

**(b)** Write `public static int getReviewCount()`.

**(c)** Add `public String getSummary()` that returns `rating + " stars by " + authorId` (an **instance** method).

**(d)** In `main` of a tester class, create 3 reviews and print `ProductReview.getReviewCount()`. Then try this line and explain the compiler error:

```java
System.out.println(ProductReview.getSummary());
```

**(e)** Answer the "Trying and thinking" slide: why *cannot* a static method invoke an instance method? Could you use `this` inside a static method?

<details markdown="block">
<summary>Answer (e)</summary>
A static method belongs to the class and runs without any object. An instance method needs a <code>this</code> — the object it works on. Inside a static method there is no current object, so there is no <code>this</code> to use; the compiler rejects it. To call an instance method from a static method you must first create (or otherwise obtain) an object and call the method on that reference.
</details>

---

## Part C: Promotions, warm-up MC
Easy
{: .label .label-green }

Given `public Polynomial(double a, double b, double c)`, which calls compile? For each that compiles, state the actual argument values used.

```java
new Polynomial(1, -5, 6);       // 1
new Polynomial(1.0, -5.0, 6.0); // 2
new Polynomial(1, 2.5, 3);      // 3
new Polynomial(1.0f, -5, 6L);   // 4
```

<details markdown="block">
<summary>Answer</summary>
1: compiles, ints promoted to (1.0, -5.0, 6.0). 2: compiles as-is. 3: compiles, (1.0, 2.5, 3.0). 4: compiles — float and long widen to double: (1.0, -5.0, 6.0).
</details>
