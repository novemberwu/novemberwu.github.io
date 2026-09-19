---
title: Lab 6 Method Signatures
layout: page
nav_order: 6
parent: 2026csa
math: mathjax
---

# Lab 6: Methods and Method Signatures
{: .no_toc }

## Goals
{: .no_toc}
* Understand what a **method** is and how it enables code reuse, procedural abstraction, and "divide and conquer" problem-solving.
* Master the anatomy of a method declaration and distinguish between a **Method Header** and a **Method Signature**.
* Learn the strict Java rules for what constitutes a method signature (crucial for the AP CSA Exam!).
* Call built-in **class (static) methods** from Java libraries, particularly the `Math` class.
* Distinguish between **void methods** (no return value) and **non-void methods** (which return a value that can be captured in a variable or expression).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: What is a Method & Why Use It?

In programming, we often find ourselves writing the exact same blocks of code repeatedly. Instead of copy-pasting code ("reinventing the wheel"), Java allows us to package a block of code, give it a name, and invoke it whenever we need it. This block is called a **method** (Slide 4).

### Key Concepts of Methods (Slide 5, 6):
* **Procedural Abstraction:** Treating a method as a "black box." The client code calling the method only needs to know **what** the method does (its inputs and output) without needing to understand **how** it is implemented internally.
* **Divide and Conquer:** Breaking a large, complex program into smaller, manageable, and testable sub-tasks.
* **Inputs and Outputs:** A method can take zero or more input arguments, can return zero or one output value, and may cause **side effects** (such as printing output to the terminal or writing to a file).

---

## Part 2: Method Headers vs. Method Signatures

Every method begins with a header that tells Java how the method can be accessed and used. However, there is a strict and crucial distinction between a **Method Header** and a **Method Signature**:

```
+----------------------------------------------------------------------------------------------------------------+
|                                              METHOD HEADER                                                     |
|                                                                                                                |
|  public static      double         calculateInterest   (double principal, double rate, int years)              |
|  [Modifiers]     [Return Type]       [Method Name]                      [Parameter List]                       |
|                                         \_______________________________________________________/              |
|                                                                     |                                          |
|                                                              v      v                                          |
|                                                      METHOD SIGNATURE                                          |
+----------------------------------------------------------------------------------------------------------------+
```

### The AP CSA Rule:
* **Method Header:** Includes access modifiers (like `public`), static modifiers (like `static`), the return type (like `double`), the method name, and the parameter list.
* **Method Signature:** Consists **ONLY** of the **Method Name** and the **Parameter List** (the types, count, and order of variables). 

| Item | In Header? | In Signature? |
| :--- | :---: | :---: |
| Access Modifiers (e.g., `public`, `private`) | **Yes** | **No** |
| Static Keyword (`static`) | **Yes** | **No** |
| Return Type (e.g., `double`, `void`, `int`) | **Yes** | **No** |
| **Method Name** | **Yes** | **Yes** |
| **Parameter Types & Order** (e.g., `(double, double, int)`) | **Yes** | **Yes** |

**Why does this matter?** Java uses the method signature to differentiate between methods. You cannot have two methods in the same class with the **same signature**—even if they have different return types or modifiers! Doing so causes a compile-time "duplicate method" error.

---

## Example A: Method Header Analysis

Easy
{: .label .label-green }

Let's look at a concrete class declaring two methods. One calculates interest, and the other prints a welcome message.

Study the code below to see how Java evaluates headers and signatures:

```java
public class FinancialPlanner {

    // Method Header: public static double calculateInterest(double principal, double rate, int years)
    // Method Signature: calculateInterest(double, double, int)
    public static double calculateInterest(double principal, double rate, int years) {
        return principal * Math.pow(1 + rate, years);
    }

    // Method Header: public static void greetUser(String name)
    // Method Signature: greetUser(String)
    public static void greetUser(String name) {
        System.out.println("Welcome, " + name + "!");
    }
}
```

---

## Task A: Method Signature Investigator (True or False)

Easy
{: .label .label-green }

Test your understanding of method signatures by answering the following **True or False** questions. These concepts are heavily tested on the AP Computer Science A exam!

### Questions:
1. **True or False:** Two methods can co-exist in the same class if they have different return types, even if their names and parameter lists are identical.
2. **True or False:** The signature of the method `public static int doMath(int x, double y)` is exactly `doMath(int, double)`.
3. **True or False:** Changing a parameter name from `int size` to `int capacity` changes the method's signature.
4. **True or False:** Two methods with signatures `doMath(double, int)` and `doMath(int, double)` have different signatures and can co-exist in the same class (this is called *overloading*).
5. **True or False:** Changing a method from `public static` to `private` changes its method signature, meaning you can now declare another method with the same name and parameters in that same class.

### Task A Solutions:
Write out your answers inside a multi-line comment block in a new file named `SignatureCheck.txt` or as a comment header in your upcoming code.

```
Question 1: FALSE (Return types are not part of the method signature in Java; duplicate signatures cause a compile error regardless of return types).
Question 2: TRUE (The method signature in Java consists strictly of the method name and the types/order of parameters).
Question 3: FALSE (Parameter names are only used locally inside the method body. Only parameter types, count, and order determine the signature).
Question 4: TRUE (Since the types of the parameters are in a different order, their signatures are different, allowing valid method overloading).
Question 5: FALSE (Access modifiers like public/private and static/non-static modifiers are not part of the method signature).
```

---

## Part 3: Calling Methods & Built-In Class Methods

We invoke (or "call") a method to execute its code block. In Java, methods that belong to a class rather than an object instance are called **class (static) methods** (Slide 12).

### Syntax for Calling Static Methods:
```java
ClassName.methodName(arguments);
```
* **Arguments:** The actual values you pass into the method during the call. They must match the type, count, and order of the parameters specified in the method's signature.
* **Defining Class Exemption:** If you are calling a static method from *inside* the same class where it is defined, writing the `ClassName.` prefix is optional.

### Void vs. Non-Void Calls (Slide 13, 14):
* **Void Method:** Has a return type of `void`. It performs an action but **returns no value**. It is called as a standalone statement.
  * *Example:* `System.out.println("Hello");`
* **Non-Void Method:** Returns a value of a specific data type. The returned value **must** be stored in a variable, printed directly, or used inside an expression.
  * *Example:* `double squareRoot = Math.sqrt(16.0);`

---

## Example B: Calling `Math` Library Methods

Medium
{: .label .label-yellow }

The Java built-in `Math` class contains a wealth of useful mathematical functions. Let's practice calling some of them:

Create a file named `MathSandbox.java` and run the code below:

```java
public class MathSandbox {
    public static void main(String[] args) {
        // Math.random() returns a double from [0.0, 1.0)
        double randVal = Math.random();
        System.out.println("Random Value: " + randVal);

        // Math.max(a, b) returns the larger of two values
        int larger = Math.max(15, 42);
        System.out.println("Larger of 15 and 42 is: " + larger);

        // Math.pow(base, exp) computes base raised to the exp power
        double cubed = Math.pow(3.0, 3.0);
        System.out.println("3 cubed is: " + cubed);
        
        // Math.sqrt(x) calculates the square root of x
        double root = Math.sqrt(25.0);
        System.out.println("Square root of 25 is: " + root);
    }
}
```

### Expected Output (Random value will vary):
```
Random Value: 0.7392144321
Larger of 15 and 42 is: 42
3 cubed is: 27.0
Square root of 25 is: 5.0
```

---

## Task B: Student Action - Quadratic Root Solver (Slide 17, 18)

Medium
{: .label .label-yellow }

Now, let's apply our knowledge of calling `Math` methods to solve an algebraic problem: finding the roots of a quadratic equation.

From algebra, any quadratic equation of the form $$x^2 + bx + c = 0$$ (assuming coefficient $$a = 1$$) has two roots:
$$x_1 = \frac{-b + \sqrt{b^2 - 4c}}{2}$$

$$x_2 = \frac{-b - \sqrt{b^2 - 4c}}{2}$$

Write a Java program that calculates and prints these two roots by calling `Math.pow()` and `Math.sqrt()`.

### Instructions:
1. Create a file named `QuadraticRoots.java`.
2. Inside the `main` method, declare and initialize two `double` variables:
   * `double b = 6.0;`
   * `double c = 5.0;`
3. Calculate the two roots $$x_1$$ and $$x_2$$ using the formulas above.
   * To calculate the discriminant ($$b^2 - 4c$$), call `Math.pow(b, 2) - 4 * c`.
   * To calculate the square root of the discriminant, call `Math.sqrt(...)` passing in your computed discriminant.
4. Print both roots to the console.

### Expected Output:
```
Equation: x^2 + 6.0x + 5.0 = 0
Root 1 (x1): -1.0
Root 2 (x2): -5.0
```

---

## Task C: General Quadratic Formula (With Coefficient $$a$$)

Hard
{: .label .label-red }

Let's extend our root solver to handle the full, general quadratic equation:
$$ax^2 + bx + c = 0$$

The complete quadratic formula is:
$$x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$

In this task, we must handle a crucial edge case: **imaginary roots**. If the discriminant ($$b^2 - 4ac$$) is negative, calling `Math.sqrt()` will return `NaN` (Not a Number). We must use a selection statement (`if-else`) to protect our program from this state!

### Instructions:
1. Open your `QuadraticRoots.java` file or create a new file named `GeneralQuadratic.java`.
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

By completing this lab, you have learned:
1. **Methods as abstractions:** Methods wrap executable blocks under a name, reducing repetition and dividing complex code.
2. **The Signature Mandate:** Java uniquely identifies methods via their **Name and Parameter List**. The return type and access modifiers do **not** affect the method's signature.
3. **Static Call Syntax:** Static methods belong to the class itself and are invoked as `ClassName.methodName(arguments)`.
4. **Guarding against Math errors:** Checking inputs (like checking if the discriminant is negative before calling `Math.sqrt()`) is a fundamental practice to avoid invalid operations such as generating `NaN`.
