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
* Understand how primitive arguments are **passed by value** in Java and track variable state changes across method boundaries (Slides 21, 22).

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

### Task A Submission:
Write out your answers (TRUE or FALSE) inside a multi-line comment block in a new file named `SignatureCheck.txt` or as a comment header in your upcoming code. Verify your logic using the rules in Part 2.

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

## Part 4: Method Arguments - Pass by Value (Slides 21, 22)

In Java, all method arguments are **passed by value**. This is a foundational concept that is heavily tested on the AP CSA exam.

### What Does "Pass by Value" Mean?
* When you pass a primitive data type (like `int`, `double`, `boolean`, `char`, etc.) to a method, Java **copies** the actual value of the variable.
* This copied value is stored in a brand-new, local parameter variable inside the method's own stack frame (Slide 27).
* Any modifications made to that parameter variable inside the method **only** affect the local copy.
* The original variable in the caller's scope (such as inside the `main` method) is completely untouched!

---

## Example C: Primitive Pass-by-Value Demonstration

Medium
{: .label .label-yellow }

Let's study a complete program that illustrates how primitive copies are modified within a method without altering the original caller's variables.

```java
public class PassByValueDemo {
    public static void main(String[] args) {
        int num = 10;
        System.out.println("Before call (main): num = " + num);
        
        // Pass num's value (10) as an argument
        changeNumber(num);
        
        System.out.println("After call (main): num = " + num);
    }

    public static void changeNumber(int x) {
        System.out.println("Inside method (start): x = " + x);
        x = 99; // Modifies the local copy 'x'
        System.out.println("Inside method (end): x = " + x);
    }
}
```

### Expected Output:
```
Before call (main): num = 10
Inside method (start): x = 10
Inside method (end): x = 99
After call (main): num = 10
```

---

## Task B: Pass-by-Value Variable Tracer

Medium
{: .label .label-yellow }

Copy/Paste Java program `ValueTracer.java` and predict what will be printed out

### Instructions:
1. Create a file named `ValueTracer.java`.
2. Inside `main`, initialize a variable `double speed = 50.0;`.
3. Call a static void method named `accelerate` passing in `speed` as an argument.
4. Inside `accelerate`, modify the parameter by adding `20.0` to it.
5. Back in `main`, print the value of `speed` to verify that it is still `50.0`.

### Source code:
```java
public class ValueTracer {
    public static void main(String[] args) {
        double speed = 50.0;
        accelerate(speed);
        System.out.println("Speed: " + speed);
    }
    public static void accelerate(double s) {
        s += 20.0;
    }
}
```

### Predict Output:
```
Speed: ?
```

---

## Wrap-Up & Summary

By completing this lab, you have learned:
1. **Methods as abstractions:** Methods wrap executable blocks under a name, reducing repetition and dividing complex code.
2. **The Signature Mandate:** Java uniquely identifies methods via their **Name and Parameter List** (types, count, and order). The return type and access modifiers do **not** affect the method's signature.
3. **Static Call Syntax:** Static (class) methods belong to the class itself (such as methods in the `Math` library) and are invoked as `ClassName.methodName(arguments)` without needing object instantiation.
4. **Pass by Value Concept:** Primitives are always passed as copies in Java, meaning their modifications inside a method never alter their original state in the caller's scope.
