---
title: Lab 7 Functions
nav_order: 3
parent: 2026 DSA
layout: page
math: mathjax


---
# Lab 7 Functions
{: .no_toc}

## Goals:
{: .no_toc}
* Review the anatomy of a method signature: name, argument list, and return type.
* Distinguish between **class (static) methods** and **instance methods**, and know when to use each.
* Understand **pass-by-value** semantics for primitives vs. reference types in Java.
* Write a basic **recursive** method using a base case and a reduction step.
* Build the mental model of "input → function → output" you'll rely on constantly once we start writing methods on data structures (linked lists, trees, etc.).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Introduction to Functions in Java

A **function** (called a **method** in Java) takes zero or more inputs (arguments), may return zero or one output value, and may cause side effects such as printing to the screen.

```
input x --> [ function f ] --> output f(x)
                 |
                 v
             side effects
```

You've already been calling built-in functions all year: `Math.sqrt()`, `Integer.parseInt()`, `Scanner.next()`. Every method has a **signature** made up of three parts:

```java
double sqrt(double a)
```
* `double` (before the name) — the **return type**
* `sqrt` — the **method name**
* `(double a)` — the **argument list** (names and types of the inputs)

There are two flavors of methods in a class:
* **Class (static) methods** — declared with the `static` keyword. Called on the *class itself* (e.g. `Math.sqrt(4.0)`). A static method can **only** access static data, never instance data.
* **Instance methods** — declared *without* `static`. Called on an *object* you construct first (e.g. `s.charAt(4)`). An instance method can access both static and instance data.

As we move into data structures, almost everything you write (`insert`, `remove`, `size`, `contains`) will be an instance method operating on an object — so getting comfortable with this distinction now matters.

---

## Example A: Calling a Class (Static) Method

Easy
{: .label .label-green }

Static methods are called directly on the class name — no object needs to be constructed first. Below, `MathAPIExample` reads two command-line arguments, `b` and `c`, and uses `Math.sqrt()` (a static method) to compute the two roots of `x² + bx + c`.

Create a file named `MathAPIExample.java` in IntelliJ and run the code below:

```java
public class MathAPIExample {
    public static void main(String[] args) {
        if (args.length < 2) {
            System.out.println("Please enter 2 numbers");
        }

        // Parse coefficients from command line
        double b = Double.parseDouble(args[0]);
        double c = Double.parseDouble(args[1]);

        // Calculate roots of x*x + b*x + c
        double discriminant = b * b - 4.0 * c;
        double d = Math.sqrt(discriminant);
        double root1 = (-b + d) / 2.0;
        double root2 = (-b - d) / 2.0;

        // print them out
        System.out.println(root1);
        System.out.println(root2);
    }
}
```

Run it with:
```
java MathAPIExample -3.0 2.0
```

### Expected Output
```
2.0
1.0
```

---

## Task A: Write Your Own Static Utility Method

Easy
{: .label .label-green }

Write a Java program named `TempConverter.java` that contains a **static class method** — not just code inside `main` — that converts a Celsius temperature to Fahrenheit, using the formula:

$$F = C \times \frac{9}{5} + 32$$

### Instructions:
1. Create a class `TempConverter`.
2. Write a **static method** with this exact signature: `public static double celsiusToFahrenheit(double celsius)`. It should return the Fahrenheit equivalent as a `double`.
3. In `main`, call `celsiusToFahrenheit(...)` three times, for `0.0`, `37.0`, and `100.0` degrees Celsius, and print each result exactly as shown below.

### Expected Output:
```
0.0 C is 32.0 F
37.0 C is 98.6 F
100.0 C is 212.0 F
```

### Code Skeleton
```java
public class TempConverter {

    // TODO 1: Complete the static method signature and body
    public static double celsiusToFahrenheit(double celsius) {
        // Your code here
        return 0.0; // Update this
    }

    public static void main(String[] args) {
        double[] tempsC = {0.0, 37.0, 100.0};

        // TODO 2: Loop through tempsC, call celsiusToFahrenheit on each,
        // and print in the format: "<C> C is <F> F"
        for (int i = 0; i < tempsC.length; i++) {
            // Your code here
        }
    }
}
```

---

## Example B: Instance Methods and Static vs. Instance Data

Medium
{: .label .label-yellow }

Instance methods live inside a class *without* the `static` keyword, and they can access both static and instance fields. Below, `totalProductsCreated` is a **static** field shared by every `Product`, while `name` and `price` are **instance** fields that belong to each individual object.

Create a file named `Product.java` and run the code below:

```java
public class Product {
    private String name;
    private double price;
    private static int totalProductsCreated = 0; // Static data member

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
        totalProductsCreated++; // Accessing and modifying static member in constructor
    }

    public void displayProductInfo() { // Instance method
        System.out.println("Product Name: " + this.name);
        System.out.println("Product Price: $" + this.price);
        System.out.println("Total Products Created: " + totalProductsCreated);
    }

    public static int getTotalProductsCreated() { // Static method to access static data
        return totalProductsCreated;
    }

    public static void main(String[] args) {
        Product product1 = new Product("Laptop", 1200.00);
        product1.displayProductInfo();

        Product product2 = new Product("Mouse", 25.00);
        product2.displayProductInfo();

        System.out.println("Total products from static method: " + Product.getTotalProductsCreated());
    }
}
```

### Expected Output
```
Product Name: Laptop
Product Price: $1200.0
Total Products Created: 1
Product Name: Mouse
Product Price: $25.0
Total Products Created: 2
Total products from static method: 2
```





---

## Example C: Pass By Value — Primitives vs. References

Medium
{: .label .label-yellow }

Java is always **pass-by-value**. But what gets copied differs depending on the type:
* **Primitives** (`int`, `double`, `boolean`, `char`, ...) — the actual *value* is copied. Changes inside the method never affect the caller's variable.
* **Reference types** (objects) — the *address* (reference) is copied. The method's local variable points to the **same object** as the caller's variable, so changes to the object's fields **are** visible to the caller. However, reassigning the local variable to a brand-new object does **not** affect the caller's variable.

Create a file named `ArgumentsPassByValue.java` and run the code below:

```java
public class ArgumentsPassByValue {

    public static void swapPrimitives(int a, int b) {
        System.out.println("Within method, before swap");
        System.out.println("a:" + a);
        System.out.println("b:" + b);
        int t = a;
        a = b;
        b = t;
        System.out.println("Within method, after swap");
        System.out.println("a:" + a);
        System.out.println("b:" + b);
    }

    public static void main(String[] args) {
        int a = 1;
        int b = 2;

        System.out.println("MAIN, before swap");
        System.out.println("a:" + a);
        System.out.println("b:" + b);

        swapPrimitives(a, b);

        System.out.println("MAIN, after swap");
        System.out.println("a:" + a);
        System.out.println("b:" + b);
    }
}
```

### Expected Output
```
MAIN, before swap
a:1
b:2
Within method, before swap
a:1
b:2
Within method, after swap
a:2
b:1
MAIN, after swap
a:1
b:2
```

Notice: even though `a` and `b` were swapped *inside* `swapPrimitives`, the variables `a` and `b` in `main` are completely untouched. That's pass-by-value for primitives.

---

## Task C: Predict, Then Verify — Reference Semantics

Medium
{: .label .label-yellow }

Below is a `Mug` class with a `contents` field, along with two static methods: `spill`, which calls a **setter** on the object it's given, and `reassignMug`, which points its local parameter at a **brand-new** `Mug` object.

### Instructions:
1. Create `PassByValueExamples.java` with the code skeleton below (it's mostly complete — the `Mug` class is done for you).
2. **Before running anything**, write your predictions as code comments directly above each `System.out.println` call in `main`, guessing what each line will print.
3. Then compile and run the program, and compare the real output to your predictions.
4. Answer this in a comment at the bottom of `main`: *why does `spill` change what `teaMug` prints, but `reassignMug` does not?*

### Code Skeleton
```java
public class PassByValueExamples {
    static class Mug {
        private String contents;

        public Mug(String contents) {
            this.contents = contents;
        }

        public void setContents(String contents) {
            this.contents = contents;
        }

        public String getContents() {
            return contents;
        }
    }

    public static void spill(Mug myMug) {
        // This modifies the object pointed to by 'myMug' (the same object
        // pointed to by 'teaMug' in main).
        myMug.setContents("nothing");
    }

    public static void reassignMug(Mug myMug) {
        // This reassigns the local 'myMug' variable to a new object.
        // It does NOT affect 'teaMug' in main.
        myMug = new Mug("new contents");
        System.out.println("Inside reassignMug method: " + myMug.getContents());
    }

    public static void main(String[] args) {
        Mug teaMug = new Mug("tea");

        // TODO 1: Predict, then print teaMug's contents BEFORE calling spill
        // Your code here

        spill(teaMug);

        // TODO 2: Predict, then print teaMug's contents AFTER calling spill
        // Your code here

        Mug coffeeMug = new Mug("coffee");

        // TODO 3: Predict, then print coffeeMug's contents BEFORE calling reassignMug
        // Your code here

        reassignMug(coffeeMug);

        // TODO 4: Predict, then print coffeeMug's contents AFTER calling reassignMug
        // Your code here

        // TODO 5: Add a comment here answering the question above
    }
}
```

---

## Example D: Recursion — Divide and Conquer

Medium
{: .label .label-yellow }

**Recursion** is a technique where a method calls *itself* to solve a smaller version of the same problem. Every correct recursive method needs two parts:
1. A **base case** — the simplest input, answered directly without another recursive call (this is what stops the recursion).
2. A **reduction step** — a call to the same method with a *smaller* input, combined with the current input to produce the answer.

Create a file named `Factorial.java` and run the code below:

```java
public class Factorial {
    public static int calculateFactorial(int n) {
        // Base case: if n is 0, return 1 (0! is 1)
        if (n == 0) {
            return 1;
        }
        // Recursive step: n * factorial of (n-1)
        else {
            return n * calculateFactorial(n - 1);
        }
    }

    public static void main(String[] args) {
        int number = 5;
        int result = calculateFactorial(number);
        System.out.println("Factorial of " + number + " is: " + result);
    }
}
```

### Expected Output
```
Factorial of 5 is: 120
```

---

## Task D: Write Your Own Recursive Method

Medium
{: .label .label-yellow }

Write a Java program named `RecursiveSum.java` that recursively sums the numbers from `1` to `n`, **without using any loop**.

### AP CSA Concept Alert: Trace the Call Stack!
Before you code, trace `recursiveSum(4)` by hand on paper: what does the method return, and what does it call next, at each step? This is the same mental trace you'll use later to trace recursive traversals of linked lists and trees — so it's worth practicing now.

### Instructions:
1. Create a class `RecursiveSum` with a `main` method.
2. Write a **recursive** static method with this exact signature: `public static int recursiveSum(int n)`.
    * **Base case:** if `n == 0`, return `0`.
    * **Reduction step:** otherwise, return `n + recursiveSum(n - 1)`.
3. In `main`, call `recursiveSum(10)` and print the result exactly as shown below.
4. Do **not** use a `for` or `while` loop anywhere in `recursiveSum` — the recursion should do all the work.

### Expected Output:
```
The sum from 1 to 10 is: 55
```

### Code Skeleton
```java
public class RecursiveSum {

    // TODO 1: Complete the recursive method
    public static int recursiveSum(int n) {
        // Base case
        if (n == 0) {
            // Your code here
        }
        // Reduction step
        else {
            // Your code here
        }
    }

    public static void main(String[] args) {
        int n = 10;
        int result = recursiveSum(n);
        System.out.println("The sum from 1 to " + n + " is: " + result);
    }
}
```

---

## Wrap-Up: Why This Matters for Data Structures

Every method you'll write on a linked list, stack, queue, or tree will be an **instance method** (operating on `this` node/structure), will need to reason carefully about **whether it's mutating the caller's object or a private copy**, and many of the classic algorithms (tree traversal, merge sort, binary search) are written **recursively**. The four skills in this lab — method signatures, static vs. instance methods, pass-by-value/reference semantics, and recursion — are the exact toolkit you'll lean on starting next lab.