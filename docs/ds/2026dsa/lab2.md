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
* Able to write program with using variables and primitive types

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Example A: Integers and Boolean

Easy
{: .label .label-green }

Write a program that determine if an integer is even or odd

Sample program
```java

// in file EvenOdd.java
public class EvenOdd{
    
    public static void main(String[] args){
        int x = 3; 
        boolean isEven = (x % 2 ) == 0;
        System.out.println(x + "is even number? " + isEven);
    }
    
}


```

## Task A: Integers and Boolean

Easy
{: .label .label-green }

Write a program that takes two positive integers as command-line
arguments and prints true if either evenly divides the other.

Sample Input
```
java TwoDivide 2 4
java TwoDivide 7 3
```
Sample Output
```
true
false
```
## Example B Integers Operators

This program evaluation expressions using variables and arithmetic operations
```java
// in file IntegerExamples

public class IntegerArithmetic{
   public static void main(String[] args){
      int x = 3; 
      int y = -2; 
      int product = x * y; 
      int sum = x + y; 
      int substraction = x - y; 
      int division = x / y; 
      int mod = x % y; 
      
      System.out.println("product:" + product);
      System.out.println("sum:" + sum);
      System.out.println("substraction:" + substraction);
      System.out.println("division:" + division);
      System.out.println("mod:" + mod);

   }
}
```
## Task B Integer Operators

Easy
{: .label .label-green }

Write a program that takes three positive integers as command-line
arguments and prints false if any one of them is greater than or equal to the sum
of the other two and true otherwise.

{: .note }
This computation tests whether the
three numbers could be the lengths of the sides of some triangle.

Sample input
```
java ThreeNumber 3 4 5
java ThreeNumber 9 20 8
```
Sample output
```
true
false
```



## Example C: Double Operators

Easy
{: .label .label-green }

Given a parabola defined in expression ax^2 + bx + c = 0. The x-coordinate of vertex 
x = \frac{-b}{2a}

```java
public class Parabola{
    
    public static void main(String[] args){
        
        double a = 1.0;
        double b = -8.0;
        double c = 15;
        
        double vertexX = -1 * b / (2 * a);
        System.out.println(vertexX);
       
    }
    
    
}


```
## Task C: Double Operations
Given a parabola defined in expression ax^2 + bx + c = 0. The y-coordinate of vertex
x = \frac{-b}{2a}

```java
public class Parabola{
    
    public static void main(String[] args){
        
        double a = 1.0;
        double b = -8.0;
        double c = 15;
        
        double vertexX = -1 * b / (2 * a);
        System.out.println(vertexX);
       
    }
    
    
}
```

## Example D: Double Precision 

This is a program to demonstrate the rounding issue of double arithmetic operation
```java
public class DoubleDemo {

    public static void main(String[] args){
        double a = 0.1 + 0.2;
        double b = 0.3;

        double epsilon = 1e-9;
      
        
        if(a == b){
            System.out.println("Equal");
        }

        System.out.println(a);
        System.out.println(b);


    }
}


```

## TASK D: Double Precision 
Modify the DoubleDemo program so that "Equal" can be printed out



## Example E: Expression evaluation
In Java the type of the result is determined by the **types of the operands** of the expression. 
```java

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













