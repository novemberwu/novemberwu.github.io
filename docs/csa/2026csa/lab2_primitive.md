---
title: Lab 2 Primitive data type
layout: page
nav_order: 2
parent: 2026csa
math: mathjax
---

# Lab 2: Primitive Data Types
{: .no_toc }

## Goals
{: .no_toc}
* Understand the three main primitive types in AP CSA: `int`, `double`, and `boolean`.
* Learn how integer division drops (truncates) fractional values.
* Master the modulo operator (`%`) to find remainders.
* Understand why decimal math (`double`) is not exact, and learn how to safely compare decimal values.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Integers (`int`) and Integer Arithmetic

In Java, an `int` represents an integer (a whole number without decimals). Computation on integers is always **exact** (Slide 18).

### The Math Operators (Slide 19)
Java uses standard symbols for basic arithmetic:
* `+` (addition)
* `-` (subtraction)
* `*` (multiplication)
* `/` (division)
* `%` (modulo / remainder)

### Truncation and the Modulo Operator
Two operators work differently in programming compared to traditional math class:

1. **Integer Division (`/`)**: When dividing two integers, Java **drops the decimal part completely** (this is called *truncation*). It does not round!
   * `5 / 3` is `1`
   * `14 / 4` is `3`
2. **Modulo (`%`)**: The modulo operator computes the **remainder** left over after integer division.
   * `5 % 3` is `2` (since 3 goes into 5 once, with a remainder of 2)
   * `14 % 4` is `2` (since 4 goes into 14 three times, with a remainder of 2)

---

## Example A: Playing with division and modulo

Easy
{: .label .label-green }

Let's see how integer division and modulo work in a simple program. 

Create a file named `IntegerPlayground.java` in IntelliJ and run the code below:

```java
public class IntegerPlayground {
    public static void main(String[] args) {
        int x = 14;
        int y = 4;

        int divisionResult = x / y; // Drops the fraction! Result: 3
        int moduloResult = x % y;   // Gets the remainder! Result: 2

        System.out.println("14 / 4 = " + divisionResult);
        System.out.println("14 % 4 = " + moduloResult);
    }
}
```

### Expected Output
```
14 / 4 = 3
14 % 4 = 2
```

---

## Task A: Modulo Magic (Unit Conversions)

Easy
{: .label .label-green }

Write a program named `ModuloFun.java` that converts a total number of inches into feet and leftover inches (1 foot = 12 inches).

### Instructions:
1. Create a class `ModuloFun` with a `main` method.
2. Declare an integer variable named `totalInches` and initialize it to `75`.
3. Calculate the number of whole feet by dividing `totalInches` by `12`. Store the result in an integer variable named `feet`. (Hint: Integer division will automatically drop the leftover inches!)
4. Calculate the remaining inches using the modulo operator `%` with `12`. Store the result in an integer variable named `inches`.
5. Print the output exactly matching the sample below.

### Expected Output (for 75 inches):
```
75 inches is equal to 6 feet and 3 inches.
```

### Code Skeleton
```java
public class ModuloFun {
    public static void main(String[] args) {
        // Declare total inches
        int totalInches = 75;

        // TODO: Calculate feet and leftover inches
        int feet = 0;   // Update this formula
        int inches = 0; // Update this formula

        // Print final result
        System.out.println(totalInches + " inches is equal to " + feet + " feet and " + inches + " inches.");
    }
}
```

---

## Part 2: Decimals (`double`) and Rounding Errors

A `double` holds decimal numbers (e.g. `3.14` or `0.1`). Unlike integers, math with `double` is **NOT exact** because computers must convert decimal numbers into binary representations (Slide 22).

### The Double Trap (Slide 24)
Because decimal calculations are not exact, very small rounding errors can occur in memory. For example:
* In standard math, $0.1 + 0.2 = 0.3$.
* In Java, `0.1 + 0.2` might result in `0.30000000000000004`!

Because of this rounding issue, **never use the `==` operator to compare two double values.** 

### The Solution: Absolute Difference
To check if two `double` values are equal, we check if the **absolute difference** between them is smaller than a tiny number (called a *tolerance* or *epsilon*, such as `0.00001`):

$$\text{Math.abs}(d_1 - d_2) < 0.00001$$

---

## Example B: Comparing Decimals Safely

Easy
{: .label .label-green }

Create a file named `DoubleTrouble.java` in IntelliJ to observe the difference between direct comparison and absolute-difference comparison:

```java
public class DoubleTrouble {
    public static void main(String[] args) {
        double d1 = 0.1 + 0.2;
        double d2 = 0.3;

        // 1. Direct comparison (Dangerous!)
        boolean isDirectEqual = (d1 == d2);
        System.out.println("Does 0.1 + 0.2 == 0.3? " + isDirectEqual); // Prints false!

        // 2. Correct comparison using absolute difference
        double difference = Math.abs(d1 - d2);
        boolean isCloseEnough = (difference < 0.00001);
        System.out.println("Are they close enough? " + isCloseEnough); // Prints true!
    }
}
```

---

## Task B: Safe Decimal Comparison

Easy
{: .label .label-green }

Write a program named `DecimalCompare.java` to practice safe double comparisons.

### Instructions:
1. Create a class `DecimalCompare` with a `main` method.
2. Declare a `double` variable named `first` and initialize it to `1.0 - 0.9` (mathematically this is `0.1`).
3. Declare another `double` variable named `second` and initialize it to `0.1`.
4. Perform a direct comparison `first == second` and store the result in a boolean variable named `isDirectEqual`. Print the result.
5. Perform a safe comparison using `Math.abs(first - second) < 0.00001` and store the result in a boolean variable named `isSafeEqual`. Print the result.

### Expected Output:
```
Direct comparison (==) result: false
Safe comparison (Math.abs) result: true
```

### Code Skeleton
```java
public class DecimalCompare {
    public static void main(String[] args) {
        double first = 1.0 - 0.9;
        double second = 0.1;

        // TODO 1: Direct comparison
        boolean isDirectEqual = false; 

        // TODO 2: Safe comparison
        boolean isSafeEqual = false; 

        System.out.println("Direct comparison (==) result: " + isDirectEqual);
        System.out.println("Safe comparison (Math.abs) result: " + isSafeEqual);
    }
}
```

---

## Part 3: Booleans and Comparisons

A `boolean` data type represents logic values and can only hold one of two values: `true` or `false` (Slide 26).

We generate booleans using comparison operators:
* `==` (equal to)
* `!=` (not equal to)
* `<` (less than)
* `>` (greater than)
* `<=` (less than or equal to)
* `>=` (greater than or equal to)

### Quick Check:
Which of these expressions evaluate to `true`?
1. `5 > 3`
2. `4 == 3 + 1`
3. `10 != 10`
4. `5 <= 5`

*(Answers: 1, 2, and 4 are `true`; 3 is `false`!)*

---

## AP CSA Primitive Data Types Cheatsheet

Here is your quick-reference summary of the primitive types and operators tested on the AP Computer Science A exam.

### The Three Core Primitive Types
* **`int`**: Represents whole numbers with no decimal points (e.g., `42`, `-7`). Arithmetic on integers is exact.
* **`double`**: Represents decimal (floating-point) numbers (e.g., `3.14`, `-0.01`). 
* **`boolean`**: Represents logic values and can only be `true` or `false`.

### Key Arithmetic Operators
* **Integer Division (`/`)**: Dividing two integers truncates (chops off) the decimal part completely. It **never** rounds.
  * `7 / 3` is `2`
  * `1 / 2` is `0`
* **Modulo (`%`)**: Returns the remainder after integer division.
  * `7 % 3` is `1` (since 3 goes into 7 twice, with a remainder of 1)
  * `10 % 10` is `0`
* **Comparison Operators**: Return a `boolean` value (`true` or `false`). Includes `==`, `!=`, `<`, `>`, `<=`, `>=`.

### The Double Trap & Safe Comparison
* Decimal math in computers is **imprecise** due to binary representation limits (e.g., `0.1 + 0.2` might equal `0.30000000000000004`).
* **Never** use `==` to compare two double values directly.
* **The Safe Comparison Formula**:
  $$\text{Math.abs}(d_1 - d_2) < \epsilon$$
  where $\epsilon$ (epsilon) is a tiny tolerance value (typically `0.00001`).

---

## Wrap-Up: Master of the Primitives

By completing this lab, you have avoided two of the most common pitfalls for AP CSA students:
* You know that integer division drops the remainder, and modulo retrieves it.
* You know how to safely handle decimal rounding errors using absolute difference.

