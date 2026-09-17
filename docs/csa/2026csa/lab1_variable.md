---
title: Lab 1 Variables and Scope
layout: page
nav_order: 1
parent: 2026csa
math: mathjax
---

# Lab 1: Variables and Scope
{: .no_toc }

## Goals
{: .no_toc}
* Understand a variable as a "named container" that holds a single value.
* Learn how to declare, initialize, and print integer (`int`) variables in Java.
* Practice updating a variable's value over time (reassignment).
* Learn the rules of variable scope (where a variable exists and can be seen).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Declaring and Updating Variables

In AP Computer Science A, we learn that a **variable** is a named location in memory (a "container") that holds a value. 

In Java, before we can use a variable, we must **declare** its type and **name** it. For example:
```java
int score; // Declaration: "Create an empty integer container named score"
score = 10; // Assignment: "Put the number 10 into score"
```
We can also do both in one line:
```java
int score = 10; // Initialization: "Create container score and put 10 inside"
```

### The "Assignment" Operator (Slide 4)
In math, $x = x + 5$ is an impossible equation. But in computer science, `=` is **not** an algebraic equals sign; it is the **assignment operator**. It means: *"evaluate the expression on the right side first, then store that final result in the variable on the left."*

For example:
```java
int q = 30;
q = 100 - q;
```
How does Java execute `q = 100 - q;`?
1. **Read**: Java looks inside `q` and gets its current value (`30`).
2. **Calculate**: Java computes `100 - 30`, which is `70`.
3. **Store**: Java writes `70` back into `q`, replacing the old value `30`.

---

## Example A: The Named Container in Action

Easy
{: .label .label-green }

Let's look at a complete Java program demonstrating variable declaration, initialization, and updates. 

Create a file named `VariableUpdate.java` in IntelliJ and paste the code below:

```java
public class VariableUpdate {
    public static void main(String[] args) {
        // Declare and initialize our container holding the value 30
        int q = 30;
        System.out.println("Initial value of q: " + q);

        // Perform named container update (Slide 4)
        q = 100 - q;

        System.out.println("Value of q after (100 - q): " + q);
    }
}
```

### Compilation & Execution
To compile and run this program in your terminal:
```bash
javac VariableUpdate.java
java VariableUpdate
```

### Expected Output
```
Initial value of q: 30
Value of q after (100 - q): 70
```

---

## Task A: The Magic Number Game

Easy
{: .label .label-green }

Write a Java program named `MagicNumber.java`. Your program will start with a secret number, perform a series of math operations on it, and print the results step-by-step.

### Instructions:
1. Create a class `MagicNumber` with a `main` method.
2. Declare an integer variable named `num` and initialize it to **any positive integer of your choice** (for example, `7`).
3. Print the starting value using `System.out.println()`.
4. Update `num` by multiplying it by 3 (i.e. `num = num * 3;`).
5. Update `num` again by adding 6 to the current value.
6. Update `num` one more time by dividing the current value by 3 (use `/`).
7. Print the final value of `num`.

### Sample Expected Output (if your starting number was 7):
```
Starting number: 7
Final magic number: 9
```

{: .note }
Can you guess why the final number is always 2 more than your starting number? Try changing your starting number to 15, run it again, and check!

### Code Skeleton
```java
public class MagicNumber {
    public static void main(String[] args) {
        // Declare and initialize variable 'num' with any positive integer
        int num = 7; 

        System.out.println("Starting number: " + num);

        // TODO: Your step-by-step container updates here
        // Step 1: Multiply num by 3
        
        // Step 2: Add 6 to num
        
        // Step 3: Divide num by 3

        // Print final output
        System.out.println("Final magic number: " + num);
    }
}
```

---

## Part 2: Understanding Variable Scope

In Java, **scope** refers to the area of code where a variable is "visible" (where it can be used).

* A variable's scope is defined by the **curly braces `{ }`** in which it is declared.
* A variable is only visible *inside* the block where it is declared, *after* its declaration line.
* If you try to use a variable outside its curly braces, the Java compiler will report a **syntax error**: "cannot find symbol."

---

## Example B: Scope and Curly Braces

Easy
{: .label .label-green }

Let's look at `ScopeDemo.java` to see what is visible and what is not.

```java
public class ScopeDemo {
    public static void main(String[] args) {
        int mainVar = 100; // This is visible throughout the entire main method

        // An if-statement block introduces new curly braces
        if (mainVar > 50) {
            int insideVar = 5; // This is only visible INSIDE these curly braces
            System.out.println("Inside: " + mainVar);   // OK! mainVar is visible here
            System.out.println("Inside: " + insideVar); // OK! insideVar is visible here
        }

        System.out.println("Outside: " + mainVar); // OK!

        // System.out.println("Outside: " + insideVar); 
        // ^ UNCOMMENTING THE LINE ABOVE WILL CAUSE A COMPILER ERROR!
        // Why? Because 'insideVar' was born and died inside the if-statement block.
    }
}
```

---

## Task B: Scope Detective

Easy
{: .label .label-green }

Below is a broken Java program named `ScopeDetective.java`. It has one **compiler syntax error** because a variable is used outside of its scope.

Your task is to copy this code, find the error, and fix it so that it compiles and runs correctly.

### Broken Code:
```java
public class ScopeDetective {
    public static void main(String[] args) {
        int startingPoints = 10;
        boolean hasBonus = true;

        if (hasBonus) {
            int bonusPoints = 5;
            System.out.println("Bonus applied!");
        }

        // ERROR: The compiler will complain on the line below!
        int totalPoints = startingPoints + bonusPoints;
        System.out.println("Total points: " + totalPoints);
    }
}
```

### Steps to Solve:
1. Create a file named `ScopeDetective.java` and paste the broken code.
2. Compile or run it. Observe the error message from the compiler (e.g. `cannot find symbol: variable bonusPoints`).
3. Fix the error by **moving the declaration** of `bonusPoints` outside the `if` block (so that it is declared before the `if` block, perhaps initialized to `0` first, and then updated inside the `if` block).
4. Verify that your fixed program compiles and prints the correct total points.

### Expected Correct Output:
```
Bonus applied!
Total points: 15
```

---

## AP CSA Variables and Scope Cheatsheet

Here is your quick-reference summary of variables and scope rules tested on the AP Computer Science A exam.

### Core Variable Concept
* **Declaration**: Introduces the variable by specifying its type and name (e.g., `int score;`). This reserves space in memory.
* **Initialization**: The first time a value is assigned to a variable (e.g., `int score = 0;`).
* **Reassignment / Value Overwriting**: Assigning a new value overwrites the old one. A variable holds **exactly one value** at a time.
* **The `=` Operator**: Not algebraic equals! Evaluates the expression on the right-hand side first, then stores the result in the variable on the left-hand side.

### Key Scope Rules
* **Definition of Scope**: The region of code where a variable is "alive" and accessible.
* **Curly Braces `{ }` block rule**: A variable is scoped to the block in which it is declared. It is created on the line of declaration and destroyed at the closing curly brace `}` of its containing block.
* **Local Variables**: Variables declared inside a method (like `main`) are local to that method and cannot be accessed outside it.
* **Compiler Error**: Attempting to use a variable outside its scope results in a `"cannot find symbol"` compiler error.

---

## Wrap-Up: Building a Solid Mental Model of Memory and Scope

By completing this lab, you have established foundational habits for AP CSA:
* You visualize variables as unique named memory containers that only change when explicitly assigned using `=`.
* You understand that curly braces `{ }` define boundaries for where variables exist, helping you prevent scope conflicts and pointer errors.

