---
title: Lab 2
nav_order: 2
parent: 2026 DSA
layout: page
math: mathjax


---
# Lab 2 Variables and Primitive Data Types
{: .no_toc}

## Goals:
{: .no_toc}
* Able to write programs using variables and primitive types.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Integer and Boolean Operations

In Java, `int` is a 32-bit signed two's complement integer. The `boolean` type represents one bit of information: `true` or `false`.

### Example A: Integers and Boolean

Easy
{: .label .label-green }

Write a program that determines if an integer is even or odd.

```java
// in file EvenOdd.java
public class EvenOdd {
    
    public static void main(String[] args){
        int x = 3; 
        boolean isEven = (x % 2 ) == 0;
        System.out.println(x + " is even number? " + isEven);
    }
}
```

### Task A: Integers and Boolean

Easy
{: .label .label-green }

Write a program `TwoDivide` that takes two positive integers as command-line arguments and prints `true` if either evenly divides the other, and `false` otherwise.

#### Sample Input
```bash
java TwoDivide 2 4
java TwoDivide 7 3
```

#### Sample Output
```
true
false
```

---

### Example B: Integer Operators

Easy
{: .label .label-green }

This program evaluates expressions using variables and basic integer arithmetic operations.

```java
// in file IntegerArithmetic.java
public class IntegerArithmetic {
   public static void main(String[] args){
      int x = 3; 
      int y = -2; 
      int product = x * y; 
      int sum = x + y; 
      int subtraction = x - y; 
      int division = x / y; 
      int mod = x % y; 
      
      System.out.println("product:" + product);
      System.out.println("sum:" + sum);
      System.out.println("subtraction:" + subtraction);
      System.out.println("division:" + division);
      System.out.println("mod:" + mod);
   }
}
```

### Task B: Integer Operators

Easy
{: .label .label-green }

Write a program `ThreeNumber` that takes three positive integers as command-line arguments and prints `false` if any one of them is greater than or equal to the sum of the other two, and `true` otherwise.

{: .note }
This computation tests whether the three numbers could be the lengths of the sides of some triangle (the **Triangle Inequality Theorem**).

#### Sample Input
```bash
java ThreeNumber 3 4 5
java ThreeNumber 9 20 8
```

#### Sample Output
```
true
false
```

---

## Part 2: Double (Floating-Point) Operations & Precision

In Java, `double` represents double-precision 64-bit IEEE 754 floating-point numbers. Unlike integers, double-precision numbers cannot represent all real numbers exactly and are subject to rounding issues.

### Example C: Double Operators

Easy
{: .label .label-green }

Given a parabola defined by the expression:
$$ax^2 + bx + c = 0$$

The x-coordinate of the vertex is given by:
$$x = \frac{-b}{2a}$$

Here is a program to compute and print the x-coordinate of the vertex:

```java
// in file Parabola.java
public class Parabola {
    
    public static void main(String[] args){
        double a = 1.0;
        double b = -8.0;
        double c = 15.0;
        
        double vertexX = -1 * b / (2 * a);
        System.out.println("Vertex X: " + vertexX);
    }
}
```

### Task C: Double Operations

Easy
{: .label .label-green }

Modify the `Parabola` program so that it computes and prints the **y-coordinate** of the vertex. 

To find the y-coordinate of the vertex, plug the x-coordinate ($$vertexX$$) back into the parabola expression:
$$vertexY = a \cdot vertexX^2 + b \cdot vertexX + c$$

```java
public class Parabola {
    
    public static void main(String[] args){
        double a = 1.0;
        double b = -8.0;
        double c = 15.0;
        
        double vertexX = -1 * b / (2 * a);
        
        // Write your code here to calculate and print vertexY
    }
}
```

---

### Example D: Double Precision 

Easy
{: .label .label-green }

This program demonstrates the rounding issues of floating-point arithmetic.

```java
// in file DoubleDemo.java
public class DoubleDemo {

    public static void main(String[] args){
        double a = 0.1 + 0.2;
        double b = 0.3;

        double epsilon = 1e-9;
        
        if (a == b) {
            System.out.println("Equal");
        }

        System.out.println("a = " + a);
        System.out.println("b = " + b);
    }
}
```

### Task D: Double Precision 

Easy
{: .label .label-green }

Modify the `DoubleDemo` program so that `"Equal"` can be successfully printed. 

*Hint:* Since floating-point representations are imprecise, you should not compare them directly using `==`. Instead, check if the absolute difference between the two numbers is smaller than a tiny threshold, which we call epsilon ($$\epsilon$$):
$$|a - b| < \text{epsilon}$$

---

### Example E: Expression Evaluation

Easy
{: .label .label-green }

In Java, the type of the result of an arithmetic operation is determined by the **types of the operands** of the expression. If you divide an integer by an integer, the result is truncated into an integer.

```java
// in file ExpressionsRunner.java
public class ExpressionsRunner {
   public static void main(String[] args) {

      int divided = 5 / 2;
      double doubleDivided = 5.0 / 2;

      System.out.println("Integer 5 / 2 is " + divided);
      System.out.println("Double 5 / 2 is " + doubleDivided);

      System.out.println(3 + 5);
      System.out.println(3 + 5.0);
      System.out.println(3.0  + 5.0);

      System.out.println("High" + 5);
      System.out.println("Version " + 2.0);
   }
}
```
