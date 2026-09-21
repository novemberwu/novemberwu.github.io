---
title: Lab 6 Method Calling
layout: page
nav_order: 6
parent: 2026csa
math: mathjax
---

# Lab 6: Calling Class Methods & the Math Library
{: .no_toc }

## Goals
{: .no_toc}
* Understand how programs are organized hierarchically into **functions, libraries, and modules** to achieve procedural abstraction.
* Master the syntax for calling **class (static) methods** in Java using the `ClassName.methodName(arguments)` pattern.
* Learn the behaviors, return types, and specific edge cases of key methods in the Java `Math` library: `Math.abs()`, `Math.pow()`, and `Math.sqrt()`.
* Deepen your understanding of **`Math.random()`** and master the mathematical formulas to scale and shift random numbers to generate values in any custom range `[start, end)`.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Functions, Libraries, and Modules

When developing large, complex software, we do not write thousands of lines of code in a single massive block. Instead, modern software development relies on **modular programming** (Slide 3):

* **Modular Programming:** The practice of organizing programs into small, independent, and self-contained modules that work together to perform a larger task. This design pattern makes code easier to test, debug, share, and reuse.
* **Function:** A named block of code that performs a specific, focused task. In Java, functions are called **methods**.
* **Library:** A set of closely related functions or methods compiled together to be used by other programs (such as the standard Java `Math` library).
* **Module:** A collection of closely related libraries, packages, and system resources (e.g., Java's standard runtime library is organized into modules like `java.base`).

By using built-in libraries like Java's `Math` class, we leverage **procedural abstraction**: we can perform complex mathematical operations (such as calculating square roots or raising numbers to powers) by simply invoking the method, without needing to know the complex algorithms used under the hood to calculate them.

---

## Part 2: Calling Class (Static) Methods

In Java, methods are generally classified into two categories: **class methods** (also called static methods) and **instance methods**. In this lab, we will focus strictly on calling **class methods** (Slide 5).

### What is a Class (Static) Method?
* Associated directly with the class itself, rather than an instance (object) of that class.
* Declared using the keyword `static` in their method header (e.g., `public static double sqrt(double a)`).
* Do **not** require you to create or instantiate an object (using the `new` keyword) before calling them.

### Syntax for Calling Class Methods (Slide 6):
```java
ClassName.methodName(argumentList);
```
* **ClassName:** The name of the class where the method is defined (e.g., `Math`).
* **methodName:** The name of the method you want to execute (e.g., `sqrt`).
* **argumentList:** A comma-separated list of actual values (or variables) you pass to the method. These must match the types, count, and order of the parameters specified in the method's signature.

> **Defining Class Exemption (Slide 6):** If you are calling a class method from *within* the same class where it is defined, writing the `ClassName.` prefix is optional. However, when calling a method from an external class (like calling a `Math` method from your custom program), the class name prefix is **required**.

---

## Part 3: Deep Dive into the `Math` Class

The built-in `java.lang.Math` library is a class containing static methods for performing basic numeric operations. Because `java.lang` is imported automatically into every Java program, you do not need to write an `import` statement to use the `Math` class.

Let's study the core static methods of the `Math` class that are essential for the AP CSA exam:

### 1. `Math.abs` (Slides 9, 10)
Calculates and returns the absolute value (magnitude without sign) of the given argument.
* **Method Signatures (Overloaded):**
  * `Math.abs(int x)`
  * `Math.abs(double x)`
  * (Also overloaded for `long` and `float`)
* **Behavior:** Returns a value of the same type as the argument.

| Method Call | Argument Type | Return Type | Result |
| :--- | :---: | :---: | :---: |
| `Math.abs(-5)` | `int` | `int` | `5` |
| `Math.abs(5)` | `int` | `int` | `5` |
| `Math.abs(-3.14)` | `double` | `double` | `3.14` |

---

### 2. `Math.pow` (Slides 11, 12)
Calculates the value of the first argument raised to the power of the second argument ($$base^{exponent}$$).
* **Method Signature:** `Math.pow(double base, double exponent)`
* **Return Type:** Always returns a **`double`**.
* **CRITICAL AP CSA TRAP:** Even if you pass integers as arguments (e.g., `Math.pow(2, 3)`), the arguments are automatically widened to doubles, and the method returns a `double` (`8.0`). Writing `int result = Math.pow(2, 3);` will cause a **compile-time error** due to a possible loss of precision unless you explicitly cast it!

```java
double powerResult = Math.pow(3.0, 4.0); // 3.0 to the power of 4.0
System.out.println(powerResult); // Prints 81.0

double intPower = Math.pow(2, 3); // Arguments widened to 2.0, 3.0
System.out.println(intPower); // Prints 8.0 (NOT 8!)
```

---

### 3. `Math.sqrt` (Slides 13, 14)
Calculates and returns the positive square root of a double value.
* **Method Signature:** `Math.sqrt(double x)`
* **Return Type:** Always returns a **`double`**.
* **Handling Negative Inputs:** You can only take the real square root of positive numbers or zero. If you pass a negative number as an argument (e.g., `Math.sqrt(-4.0)`), Java cannot calculate a real value. Instead of throwing an error or crashing, the method returns a special constant value: **`Double.NaN`** (which stands for **Not a Number**).

```java
double rootOfThree = Math.sqrt(3.0);
System.out.println(rootOfThree); // Prints 1.7320508075688772 (Slide 14)

double rootOfNegative = Math.sqrt(-9.0);
System.out.println(rootOfNegative); // Prints NaN (Slide 14)
```

---

## Part 4: Generating Random Numbers with `Math.random`

One of the most widely used methods on the AP CSA exam is `Math.random()` (Slide 15).

* **Method Signature:** `Math.random()`
* **Return Type:** `double`
* **Range:** Returns a pseudorandom `double` in the range **`[0.0, 1.0)`** (half-open interval).
  - This means the generated number can be exactly `0.0` (inclusive), but is strictly less than `1.0` (exclusive).

$$\text{Range: } 0.0 \le \text{Math.random()} < 1.0$$

### Scaling and Shifting to a Custom Range `[start, end)` (Slide 16)
To generate a random decimal value in a custom range from `start` (inclusive) to `end` (exclusive), we apply a two-step mathematical formula:

1. **Scale (Multiply):** Multiply `Math.random()` by the width of the interval, which is `(end - start)`. This changes the range from `[0.0, 1.0)` to `[0.0, end - start)`.
2. **Shift (Add):** Add `start` to the scaled value. This shifts the range from `[0.0, end - start)` to the desired `[start, end)`.

$$\text{Random Value} = \text{start} + \text{Math.random()} \times (\text{end} - \text{start})$$

Let's trace this formula step-by-step for the range `[5.0, 15.0)` where `start = 5.0` and `end = 15.0`:
* Width of range: `15.0 - 5.0 = 10.0`
* Scaled value: `Math.random() * 10.0` (range is `[0.0, 10.0)`)
* Shifted value: `5.0 + Math.random() * 10.0` (range is `[5.0, 15.0)`)

```java
double start = 5.0;
double end = 15.0;
double randomInRange = start + Math.random() * (end - start);
System.out.println(randomInRange); // Generates a value like 8.435792
```

---

## Example A: Absolute Difference Analysis

Easy
{: .label .label-green }

Let's study how calling `Math.abs` can be used to compare two values or calculate physical distance in a coordinate system.

```java
public class DistanceCalculator {
    public static void main(String[] args) {
        int homePosition = 12;
        int schoolPosition = 45;
        int distance = Math.abs(homePosition - schoolPosition);
        System.out.println("Distance: " + distance + " blocks.");
    }
}
```

---

## Task A: Find Min and Max Without Branching

Easy
{: .label .label-green }

Barring numeric overflow, we can compute the maximum and minimum of two integers `a` and `b` mathematically without using conditional statements (`if-else`), relational operators, or the built-in `Math.max()` and `Math.min()` methods.

The mathematical formulas are:

$$\text{Maximum} = \frac{a + b + |a - b|}{2}$$

$$\text{Minimum} = \frac{a + b - |a - b|}{2}$$

Your task is to write a complete Java program `MinMaxFinder.java` that computes their maximum and minimum using the formulas above in **under 10 lines of code**.

### Code Skeleton:
```java
public class MinMaxFinder {
    public static void main(String[] args) {
        int a = 17, b = 42;
        // TODO: Calculate max and min without using Math.max() or if statements
        int max = 0; // Replace with formula
        int min = 0; // Replace with formula
        System.out.println("Max: " + max + ", Min: " + min);
    }
}
```

### Expected Output:
```
Max: 42, Min: 17
```

---

## Example B: Simulating a Dice Roll with `Math.random`

Medium
{: .label .label-yellow }

Let's look at how we can generate integer random numbers, such as a roll of a standard six-sided die (values `1` through `6`).

```java
public class DiceRoller {
    public static void main(String[] args) {
        int min = 1, max = 6;
        int dieRoll = min + (int)(Math.random() * (max - min + 1));
        System.out.println("You rolled a: " + dieRoll);
    }
}
```

---

## Task B: Custom Range Random Integer Generator 

Medium
{: .label .label-yellow }

Write a complete program `CustomRandom.java` that takes a start and an end value, and produces a random floating-point number in the range `[start, end)` in **under 10 lines of code**.

### Code Skeleton:
```java
public class CustomRandom {
    public static void main(String[] args) {
        int start = 12;
        int end = 18;
        // TODO: Generate a random int in the range [start, end]
        int rand = 0.0; // Replace with formula
        System.out.println("Random: " + rand);
    }
}
```

### Expected Output (individual number will vary):
```
Random: 15
```

---

## Task C: Triangle Area via Heron's Formula

Hard
{: .label .label-red }

Write a complete program `TriangleArea.java` that calculates the area of a triangle given its three side lengths `a`, `b`, and `c` using **Heron's Formula** in **under 10 lines of code**:

$$\text{Semi-perimeter (s)} = \frac{a + b + c}{2}$$

$$\text{Area} = \sqrt{s(s-a)(s-b)(s-c)}$$

*Note:* If the side lengths are mathematically invalid and cannot form a triangle, the term inside `Math.sqrt()` will be negative, naturally producing a result of `NaN` (Not a Number) as discussed in Slide 14!

### Code Skeleton:
```java
public class TriangleArea {
    public static void main(String[] args) {
        double a = 3, b = 4, c = 5;
        // TODO 1: Calculate the semi-perimeter s
        double s = 0.0; // Replace with formula
        // TODO 2: Calculate area using Heron's Formula and Math.sqrt
        double area = 0.0; // Replace with formula
        System.out.println("Area: " + area);
    }
}
```

### Expected Output:
```
Area: 6.0
```

*Challenge:* Change `c = 5` to `c = 10` (an invalid triangle). Note how the output naturally becomes `Area: NaN` without needing any verbose conditional checks!

---

## Task D: General Quadratic Formula (With Coefficient $$a$$)

Hard
{: .label .label-red }

Let's extend our root solver to handle the full, general quadratic equation:
$$ax^2 + bx + c = 0$$

The complete quadratic formula is:
$$x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$

In this task, we must handle a crucial edge case: **imaginary roots**. If the discriminant ($$b^2 - 4ac$$) is negative, calling `Math.sqrt()` will return `NaN` (Not a Number). We must use a selection statement (`if-else`) to protect our program from this state!

### Instructions:
1. Create a file named `GeneralQuadratic.java`.
2. Inside `main`, initialize three double coefficients:
   * `double a = 1.0;`
   * `double b = -5.0;`
   * `double c = 6.0;`
3. Calculate the discriminant: $$discriminant = b^2 - 4ac$$.
4. Implement a conditional check:
   * **If the discriminant is negative** ($$< 0$$), print: `"The equation has no real roots."`
   * **Otherwise**, calculate the two real roots $$x_1$$ and $$x_2$$ using the full formula and print them:
     $$x_1 = \frac{-b + \sqrt{discriminant}}{2a}$$
     $$x_2 = \frac{-b - \sqrt{discriminant}}{2a}$$
5. Test your program with two scenarios:
   * **Scenario 1 (Real Roots):** `a = 1.0`, `b = -5.0`, `c = 6.0`
   * **Scenario 2 (No Real Roots):** `a = 2.0`, `b = 1.0`, `c = 3.0`

### Expected Output:

**Scenario 1:**
```
Coefficients: a=1.0, b=-5.0, c=6.0
Root 1 (x1): 3.0
Root 2 (x2): 2.0
```

**Scenario 2:**
```
Coefficients: a=2.0, b=1.0, c=3.0
The equation has no real roots.
```

### Code Skeleton:
```java
public class GeneralQuadratic {
    public static void main(String[] args) {
        // Coefficients
        double a = 1.0;
        double b = -5.0;
        double c = 6.0;

        // TODO 1: Calculate the discriminant (b^2 - 4ac)
        double discriminant = 0.0; // Replace with code

        System.out.println("Coefficients: a=" + a + ", b=" + b + ", c=" + c);

        // TODO 2: Use if-else to check for negative discriminant
        if (discriminant < 0) {
            // Print message
        } else {
            // Calculate and print Root 1 and Root 2
        }
    }
}
```

---

## Wrap-Up & Summary

By completing this lab, you have mastered:
1. **Hierarchical Code Organization:** The relationship between functions/methods, libraries, and modules, and how they help structure code.
2. **Static Method Calling:** Calling class methods utilizing `ClassName.methodName(arguments)` without needing object instantiation.
3. **Core `Math` Methods:**
   * `Math.abs()` to obtain the positive magnitude of any numeric value.
   * `Math.pow()` to compute base to power, keeping in mind that it **always returns a double**.
   * `Math.sqrt()` to compute positive square roots, and understanding how negative inputs naturally produce `NaN`.
4. **Custom Random Ranges:** Implementing scaling and shifting formulas to transform `Math.random()`'s default `[0.0, 1.0)` range into any range `[start, end)`.
