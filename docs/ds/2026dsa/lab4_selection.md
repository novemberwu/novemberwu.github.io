---
title: Lab 4
nav_order: 2
parent: 2026 DSA
layout: page
math: mathjax


---
# Lab 4 Conditional Statements and Nested If
{: .no_toc}

## Goals:
{: .no_toc}
* Understand conditional execution flow using `if`, `if-else`, and `else if` chains.
* Master nested `if` statements and trace nested conditional logic.
* Learn logical operators (`&&`, `||`, `!`) and understand short-circuit evaluation.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Example A: Simple If-Else and Logical Operators

Easy
{: .label .label-green }

A basic conditional statement in Java allows your program to make decisions. The block of code inside the `if` statement is executed if and only if the boolean expression evaluates to `true`.

```java
// in file AgeCheck.java
public class AgeCheck {
    
    public static void main(String[] args) {
        int age = 17; 
        
        if (age < 18) {
            System.out.println("Age is " + age + ": Eligible for student discount!");
        } else {
            System.out.println("Age is " + age + ": Standard ticket price.");
        }
    }
}
```

---

## Task A: Odd/Even & Sign Check

Easy
{: .label .label-green }

Write a program `SignCheck` that takes a single integer as a command-line argument. The program should evaluate the number and print one of the following:
* `"Positive Even"` if the number is positive and evenly divisible by 2.
* `"Positive Odd"` if the number is positive but not evenly divisible by 2.
* `"Negative"` if the number is less than 0.
* `"Zero"` if the number is exactly 0.

{: .note }
To parse an integer from command-line arguments, use `Integer.parseInt(args[0])`.

### Sample Input
```bash
java SignCheck 4
java SignCheck 7
java SignCheck -3
java SignCheck 0
```

### Sample Output
```
Positive Even
Positive Odd
Negative
Zero
```

---

## Example B: Multi-Way Branching with Else-If

Easy
{: .label .label-green }

When you have multiple mutually exclusive conditions, use an `if-else if-else` chain. Java evaluates conditions from top to bottom. As soon as one condition evaluates to `true`, its corresponding block executes, and the rest of the chain is skipped.

```java
// in file GradeScale.java
public class GradeScale {
    
    public static void main(String[] args) {
        double score = 88.5;
        
        if (score >= 90.0) {
            System.out.println("Grade: A");
        } else if (score >= 80.0) {
            System.out.println("Grade: B");
        } else if (score >= 70.0) {
            System.out.println("Grade: C");
        } else {
            System.out.println("Grade: F");
        }
    }
}
```

{: .note }
The order of conditions in an `else if` chain is critical! If you rewrite the conditions in the wrong order (e.g., checking `score >= 70.0` first), any score above 70 (like `95.0`) will trigger the first matching condition and incorrectly print `"Grade: C"`.

---

## Task B: Triangle Classifier

Easy
{: .label .label-green }

In Lab 2 Task B, you wrote a program to check if three side lengths could form a triangle. Now, let's expand that program to classify the triangle type.

Write a program `TriangleClassifier` that takes three positive integers as command-line arguments (representing the lengths of three sides: `a`, `b`, and `c`).

1. First, check if the three side lengths can form a valid triangle. A valid triangle must satisfy the **Triangle Inequality Theorem**: the sum of any two sides must be strictly greater than the third side ($$a + b > c$$ AND $$a + c > b$$ AND $$b + c > a$$).
   * If the side lengths **cannot** form a valid triangle, print `"Invalid Triangle"`.
2. If they can form a valid triangle, classify the triangle into one of three categories:
   * If all three sides are equal, print `"Equilateral"`.
   * If exactly two sides are equal, print `"Isosceles"`.
   * If no two sides are equal, print `"Scalene"`.

### Sample Input
```bash
java TriangleClassifier 3 3 3
java TriangleClassifier 3 4 5
java TriangleClassifier 5 5 8
java TriangleClassifier 1 2 10
```

### Sample Output
```
Equilateral
Scalene
Isosceles
Invalid Triangle
```

---

## Example C: Short-Circuit Evaluation

Moderate
{: .label .label-purple }

Java uses **short-circuit evaluation** for the logical operators `&&` (AND) and `||` (OR):
* For `&&`: If the left-hand operand is `false`, the overall expression must be `false`. Therefore, Java **skips** evaluating the right-hand operand.
* For `||`: If the left-hand operand is `true`, the overall expression must be `true`. Therefore, Java **skips** evaluating the right-hand operand.

Short-circuiting is extremely useful for guarding against operations that could crash your program (like division by zero or a null reference pointer).

```java
public class ShortCircuitDemo {
    
    public static void main(String[] args) {
        int x = 0;
        
        // SAFE: Since x == 0 is true, the left side of || is true.
        // Java short-circuits and skips evaluating (10 / x > 2),
        // preventing an ArithmeticException (Division by zero).
        if (x == 0 || 10 / x > 2) {
            System.out.println("Safe condition 1 executed successfully.");
        }
        
        // SAFE: Since x != 0 is false, the left side of && is false.
        // Java short-circuits and skips (10 / x > 2).
        if (x != 0 && 10 / x > 2) {
            System.out.println("This won't print, but it also won't crash!");
        }
    }
}
```

---

## Task C: Safe Divisibility Tester

Moderate
{: .label .label-purple }

Write a program `SafeDivisible` that takes two integers as command-line arguments: `numerator` and `denominator`.

We want to check if the `numerator` is evenly divisible by the `denominator` (i.e., `numerator % denominator == 0`). However, if the `denominator` is `0`, performing `%` will cause your program to crash with an `ArithmeticException`.

You must implement a safe conditional check in your program using **short-circuit evaluation** in a single `if` statement:
1. If the `denominator` is `0`, print `"Cannot divide by zero!"`.
2. If the `denominator` is not `0` and the `numerator` is divisible by the `denominator`, print `"Divisible!"`.
3. If the `denominator` is not `0` and the `numerator` is not divisible, print `"Not divisible."`.

### Sample Input
```bash
java SafeDivisible 12 3
java SafeDivisible 12 5
java SafeDivisible 12 0
```

### Sample Output
```
Divisible!
Not divisible.
Cannot divide by zero!
```

---

## Example D: Nested Conditional Structures

Moderate
{: .label .label-purple }

An `if-else` structure can be nested inside another `if-else` structure to represent hierarchical decisions.

```java
// in file NestedDemo.java
public class NestedDemo {
    
    public static void main(String[] args) {
        boolean isWeekend = true;
        boolean isRainy = false;
        
        if (isWeekend) {
            if (isRainy) {
                System.out.println("Stay inside and play video games!");
            } else {
                System.out.println("Go outside for a nice walk!");
            }
        } else {
            System.out.println("Time to go to school and study.");
        }
    }
}
```

---

## Task D: Leap Year Validator (Complex Conditional Logic)

Hard
{: .label .label-red }

Write a program `LeapYear` that takes an integer `year` as a command-line argument and determines whether it is a leap year.

A year is a leap year if and only if it satisfies the following rules:
1. It is divisible by 4.
2. However, if it is divisible by 100, it must **also** be divisible by 400 to be a leap year.

### Leap Year Rules Examples:
* `2024` is divisible by 4 but not by 100 $$\rightarrow$$ **Leap Year**.
* `1900` is divisible by 4 and by 100, but not divisible by 400 $$\rightarrow$$ **NOT a Leap Year**.
* `2000` is divisible by 4, 100, and 400 $$\rightarrow$$ **Leap Year**.

### Your Challenge:
Implement **two different approaches** inside your `LeapYear` class to verify they print the exact same result:

1. **Approach 1 (Nested Ifs)**: Solve this using only nested `if` and `if-else` statements. You are **not** allowed to use the logical operators `&&` or `||` for this approach.
2. **Approach 2 (Single Condition)**: Solve this using a single `if-else` statement with a complex boolean expression using logical operators (`&&`, `||`, `!`).

### Sample Input
```bash
java LeapYear 2024
java LeapYear 1900
java LeapYear 2000
```

### Sample Output
```
--- Testing Year: 2024 ---
(Approach 1) 2024 is a leap year.
(Approach 2) 2024 is a leap year.

--- Testing Year: 1900 ---
(Approach 1) 1900 is NOT a leap year.
(Approach 2) 1900 is NOT a leap year.

--- Testing Year: 2000 ---
(Approach 1) 2000 is a leap year.
(Approach 2) 2000 is a leap year.
```
