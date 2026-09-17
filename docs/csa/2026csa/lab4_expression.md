---
title: Lab 4 Expression
layout: page
nav_order: 4
parent: 2026csa
math: mathjax
---

# Lab 4: Expressions and Assignment
{: .no_toc }

## Goals
{: .no_toc}
* Understand that an **expression** is a programming construct that evaluates to a single value.
* Master **assignment expressions** (`=`) and understand how variable reassignment overwrites previous values.
* Learn and apply **arithmetic operator precedence** rules (PEMDAS).
* Use **compound assignment operators** (`+=`, `-=`, `*=`, `/=`, `%=`) and **post-increment/decrement operators** (`++`, `--`).
* Understand **arithmetic result types** and how Java performs **automatic type conversion** and **promotion**.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Expressions and Assignment Expressions

An **Expression** is a programming construct that, when evaluated, produces a single value (Slide 4). 

An **assignment expression** is a type of expression that assigns a value to a variable using the assignment operator (`=`) (Slide 5). The general format is:

```java
variable = expression;
```

### The Evaluation Rule
The right-hand side (RHS) of the assignment operator is evaluated completely to a single value **before** that value is stored in the variable on the left-hand side (LHS).

Let's look at how a variable's value updates in memory (Slide 6):
1. `int x = 10;` — Declares variable `x` and initializes it to `10`.
2. `x = 12;` — Evaluates the right-hand side (`12`) and assigns it to `x`. This overwrites the old value of `10`.
3. `x = x + 2;` — Retrieves the current value of `x` (`12`), adds `2` to it to yield a new value of `14`, and then assigns `14` back to `x`. This overwrites the value `12`.

---

## Example A: Tracing Assignment Expressions

Easy
{: .label .label-green }

Let's observe how variables hold only one value at a time, and how reassigning them overwrites their previous contents.

Create a file named `AssignmentPlayground.java` in IntelliJ and run the code below:

```java
public class AssignmentPlayground {
    public static void main(String[] args) {
        int x = 5;
        int y = 10;

        // Overwriting x's value
        x = 20; 

        // RHS evaluates first: y is 10, x is 20, so 10 + 20 = 30 is assigned to y
        y = y + x; 

        // Assigning one variable to another duplicates the value, it does NOT link them!
        x = y; 
        y = 50; // Changing y now does NOT affect x

        System.out.println("Value of x: " + x);
        System.out.println("Value of y: " + y);
    }
}
```

### Expected Output:
```
Value of x: 30
Value of y: 50
```

---

## Task A: Tracing and Updating a Savings Ledger

Easy
{: .label .label-green }

In this task, you will write a program that simulates a bank account's savings ledger. You will practice declaring, initializing, and reassigning variables to track financial transactions over time.

### Instructions:
1. Create a Java file named `SavingsLedger.java`.
2. Inside `main`, perform the following steps in sequence:
   * Declare an integer variable named `balance` and initialize it to `1000` (your starting savings).
   * You deposit some cash: increase the `balance` by `250` by reassigning `balance = balance + 250;`.
   * You buy a new keyboard: decrease the `balance` by `80` by reassigning `balance` to subtract `80`.
   * Your bank awards you a bonus: update the `balance` by doubling it (multiply by `2`).
   * Print the final value of the `balance` variable exactly in the format: `"Final balance: $[value]"`.

### Expected Output:
```
Final balance: $2340
```

### Code Skeleton:
```java
public class SavingsLedger {
    public static void main(String[] args) {
        // TODO 1: Declare and initialize the balance variable to 1000

        // TODO 2: Increase balance by 250 (deposit)

        // TODO 3: Decrease balance by 80 (keyboard purchase)

        // TODO 4: Double the balance (bank bonus)

        // TODO 5: Print the final balance in the specified format
    }
}
```

---

## Part 2: Arithmetic Precedence (Order of Operations)

When an expression contains multiple arithmetic operators, Java evaluates them in a specific order of precedence (Slide 10):

1. **Parentheses `()`**: Operations inside parentheses are always evaluated first.
2. **Multiplicative operators (`*`, `/`, `%`)**: Multiplication, division, and modulo have equal precedence and are evaluated next.
3. **Additive operators (`+`, `-`)**: Addition and subtraction have equal precedence and are evaluated last.

### Associativity Rule (Left-to-Right)
If operators of the **same precedence level** appear in an expression, Java evaluates them **from left to right**. 

For example, in `10 - 5 - 2`, since `-` has equal precedence, it is evaluated as `(10 - 5) - 2` which is `3` (not `10 - (5 - 2)` which is `7`).

---

## Example B: Precedence Play

Easy
{: .label .label-green }

Let's analyze how standard math precedence rules determine the output of mixed arithmetic expressions (Slide 11).

Create a file named `PrecedencePlay.java` in IntelliJ and run the code below:

```java
public class PrecedencePlay {
    public static void main(String[] args) {
        // 1. evaluated as 10 - (2 * 3) - 1 => 10 - 6 - 1 => 3
        int result1 = 10 - 2 * 3 - 1; 

        // 2. evaluated as 10 - (5 * 2) => 10 - 10 => 0
        int result2 = 10 - 5 * 2; 

        // 3. evaluated as 2 - (4 * 4) => 2 - 16 => -14
        int result3 = 2 - 4 * 4; 

        // 4. evaluated as ((10 / 3) * 3) - 8 => (3 * 3) - 8 => 9 - 8 => 1
        // Note: 10 / 3 is 3 due to integer truncation!
        int result4 = 10 / 3 * 3 - 8; 

        System.out.println(result1);
        System.out.println(result2);
        System.out.println(result3);
        System.out.println(result4);
    }
}
```

### Expected Output:
```
3
0
-14
1
```

---

## Task B: Restructuring with Parentheses

Easy
{: .label .label-green }

By default, Java evaluates arithmetic operators according to PEMDAS rules. However, you can use parentheses `()` to override standard precedence and dictate which operations occur first.

In this task, you are given three expressions. Your job is to add parentheses to each expression so that they evaluate to the target outputs.

### Instructions:
1. Create a class named `ParenthesesPractice` in a file named `ParenthesesPractice.java`.
2. Copy the following code into your `main` method.
3. Modify **only** the lines for `result1`, `result2`, and `result3` by inserting parentheses `()` to achieve the specified target values.

### Code Skeleton:
```java
public class ParenthesesPractice {
    public static void main(String[] args) {
        // Target Output for result1: 20
        // Currently evaluates to 14 because of 3 * 4 + 2 = 12 + 2 = 14
        int result1 = 2 * 3 + 4; // Add parentheses here

        // Target Output for result2: 1
        // Currently evaluates to 12 because of 16 - 8 / 2 = 16 - 4 = 12
        int result2 = 16 - 8 / 8; // Add parentheses here

        // Target Output for result3: 15
        // Currently evaluates to 11 because of 15 / 5 + 2 * 4 = 3 + 8 = 11
        int result3 = 15 / 5 + 2 * 4; // Add parentheses here

        System.out.println("Result 1: " + result1);
        System.out.println("Result 2: " + result2);
        System.out.println("Result 3: " + result3);
    }
}
```

### Expected Output:
```
Result 1: 20
Result 2: 1
Result 3: 15
```

---

## Part 3: Compound Assignment and Increment/Decrement Operators

In programming, we frequently update a variable relative to its current value (e.g., adding `1` to a score, or doubling a price). Java provides elegant shorthands to write these operations concisely.

### Compound Assignment Operators (Slide 12)
These operators combine an arithmetic operation with assignment:

| Shorthand | Equivalent Expression | Meaning |
| :--- | :--- | :--- |
| `x += y;` | `x = x + y;` | Add `y` to `x` and store in `x` |
| `x -= y;` | `x = x - y;` | Subtract `y` from `x` and store in `x` |
| `x *= y;` | `x = x * y;` | Multiply `x` by `y` and store in `x` |
| `x /= y;` | `x = x / y;` | Divide `x` by `y` and store in `x` |
| `x %= y;` | `x = x % y;` | Find remainder of `x / y` and store in `x` |

### Post-Increment and Decrement Operators (Slide 13)
For the extremely common task of adding or subtracting exactly `1`, Java provides:
* **Post-Increment (`x++`)**: Equivalent to `x += 1;` or `x = x + 1;`
* **Post-Decrement (`x--`)**: Equivalent to `x -= 1;` or `x = x - 1;`

{: .note }
The "post" means that in a larger expression, the variable's current value is used first, and then it is incremented/decremented. For simple standalone statements (like `x++;`), it simply increases or decreases the variable by 1.

---

## Example C: Shorthand in Action

Easy
{: .label .label-green }

Let's explore how compound assignment and increment/decrement operators shorten our code.

Create a file named `ShorthandDemo.java` and run the code below:

```java
public class ShorthandDemo {
    public static void main(String[] args) {
        int score = 0;
        System.out.println("Start: " + score);

        score += 10; // score is now 10
        System.out.println("After += 10: " + score);

        score *= 3;  // score is now 30
        System.out.println("After *= 3: " + score);

        score++;     // score is now 31
        System.out.println("After increment: " + score);

        score %= 5;  // score is now 1 (remainder of 31 / 5)
        System.out.println("After %= 5: " + score);
    }
}
```

### Expected Output:
```
Start: 0
After += 10: 10
After *= 3: 30
After increment: 31
After %= 5: 1
```

---

## Task C: Arcade Game Tracker

Easy
{: .label .label-green }

You are designing the back-end system for an retro arcade game. Write a program named `ArcadeTracker.java` that keeps track of a player's points and multiplier during a round.

### Instructions:
1. Create a class named `ArcadeTracker` with a `main` method.
2. Initialize two integer variables:
   * `score` to `0`
   * `multiplier` to `1`
3. Use compound assignment and increment operators to reflect the following gameplay events in order:
   * **Start Game**: Print the starting score and multiplier.
   * **Event 1 (Coin collected)**: Increase `score` by `100` points.
   * **Event 2 (Power-up grabbed)**: Increment `multiplier` by `1` using the `++` operator.
   * **Event 3 (Monster defeated)**: Multiply your current `score` by `multiplier`.
   * **Event 4 (Critical strike hit)**: Increase `score` by `500` points.
   * **Event 5 (Shield broken)**: Decrement `multiplier` by `1` using the `--` operator.
4. Print the final `score` and `multiplier` in the console.

### Expected Output:
```
Arcade Start - Score: 0, Multiplier: 1
End Round - Score: 700, Multiplier: 1
```

### Code Skeleton:
```java
public class ArcadeTracker {
    public static void main(String[] args) {
        // TODO 1: Initialize score to 0 and multiplier to 1

        // Print initial state
        System.out.println("Arcade Start - Score: " + score + ", Multiplier: " + multiplier);

        // TODO 2: Coin collected (add 100 to score)

        // TODO 3: Power-up grabbed (increment multiplier by 1)

        // TODO 4: Monster defeated (multiply score by the multiplier)

        // TODO 5: Critical strike hit (add 500 to score)

        // TODO 6: Shield broken (decrement multiplier by 1)


        // Print final state
        System.out.println("End Round - Score: " + score + ", Multiplier: " + multiplier);
    }
}
```

---

## Part 4: Arithmetic Operation Result Type & Type Conversion

When Java evaluates an arithmetic operation, the data types of the operands determine the data type of the resulting value (Slide 15-17):

1. **`int` and `int` $$\rightarrow$$ `int`**: If both values are integers, the result is always an integer. Any fractional part is truncated.
2. **`double` and `double` $$\rightarrow$$ `double`**: If both values are doubles, the result is a double.
3. **`double` and `int` $$\rightarrow$$ `double`**: If at least one operand is a `double`, Java automatically converts (promotes) the integer to a double before performing the operation.

### Automatic Type Conversion and Promotion (Slide 18)
When multiple data types exist in an expression, Java performs widening conversions automatically to prevent loss of precision:

* **String Concatenation Promotion**: When a `String` is added (`+`) to any numeric type, the number is automatically converted to a `String`.
  * `"x:" + 99` $$\rightarrow$$ Evaluates to the String `"x:99"`
* **Numeric Widening**: When an `int` is mixed with a `double` in math operations, the `int` is promoted to a `double`.
  * `11 * 0.25` $$\rightarrow$$ Since `11` is an `int` and `0.25` is a `double`, `11` is promoted to `11.0`, resulting in a `double` value of `2.75`.

---

## Example D: Type Conversions and Widening

Easy
{: .label .label-green }

Let's examine how Java decides the types of results based on operands.

Create a file named `ResultTypes.java` and run the code below:

```java
public class ResultTypes {
    public static void main(String[] args) {
        int a = 5;
        int b = 2;
        double x = 2.0;

        // 1. int divided by int yields int (truncated!)
        int intDiv = a / b; 
        System.out.println("5 / 2 = " + intDiv);

        // 2. int divided by double yields double (no truncation!)
        // b is promoted to double first
        double mixedDiv = a / x; 
        System.out.println("5 / 2.0 = " + mixedDiv);

        // 3. String concatenation promotion
        String message = "Score is: " + 95; // 95 is promoted to String
        System.out.println(message);
    }
}
```

### Expected Output:
```
5 / 2 = 2
5 / 2.0 = 2.5
Score is: 95
```

---

## Task D: Course Grade Average Calculator

Easy
{: .label .label-green }

In AP CSA, a common mistake is dividing a sum of integers by an integer count, which truncates the decimal part (e.g., `85 + 90 + 88 = 263`, then `263 / 3 = 87` instead of `87.6666...`).

Write a program named `GradeAverage.java` that correctly computes a student's double grade average from three integer test scores by exploiting the automatic type promotion rule.

### Instructions:
1. Create a class named `GradeAverage` in a file named `GradeAverage.java`.
2. Define three integer variables:
   * `test1 = 88`
   * `test2 = 91`
   * `test3 = 96`
3. Declare an integer variable named `sum` that stores the sum of the three test scores.
4. Declare a double variable named `average`. Compute the average score by dividing `sum` by a double literal `3.0` (instead of the integer `3`).
5. Print the results in the console in the exact format shown in the expected output below.

{: .note }
Dividing the integer `sum` by `3.0` promotes `sum` to a `double` before division, giving you an exact decimal result!

### Expected Output:
```
Test Sum: 275
Course Average: 91.66666666666667
```

### Code Skeleton:
```java
public class GradeAverage {
    public static void main(String[] args) {
        // TODO 1: Declare and initialize the three test scores (88, 91, 96)

        // TODO 2: Calculate the sum of the tests and store it in an integer variable 'sum'

        // TODO 3: Calculate the average by dividing 'sum' by 3.0 to get a double precision result
        double average = 0.0; // Update this

        // TODO 4: Print the sum and average in the required format
        System.out.println("Test Sum: " + sum);
        System.out.println("Course Average: " + average);
    }
}
```

---

## AP CSA Expressions & Assignment Cheatsheet

Here is your quick-reference summary of expression rules tested on the AP Computer Science A exam.

### Definition of an Expression
* Every expression **must evaluate to a single value** of a specific type.
* In `int a = 5 * 2 + 3;`, the expression on the right-hand side is evaluated first to `13` before being stored in `a`.

### Precedence Hierarchy (PEMDAS)
1. **Parentheses `()`**: Highest precedence.
2. **Multiplicative (`*`, `/`, `%`)**: Medium precedence. Evaluated left-to-right.
3. **Additive (`+`, `-`)**: Lowest precedence. Evaluated left-to-right.

### Shortcut Operators
* **Compound Assignment (`+=`, `-=`, `*=`, `/=`, `%=`)**: Performs math and reassigns in a single step (e.g., `x *= 2` is `x = x * 2`).
* **Post-Increment/Decrement (`++`, `--`)**: Increases or decreases a variable by exactly `1`.

### Data Type Promotion Rules
* **`int` with `int`**: Math result is always `int` (fractions are truncated, e.g., `7 / 3` is `2`).
* **`int` with `double`**: The `int` is promoted to `double`, and the math result is `double` (e.g., `7 / 3.0` is `2.3333...`).
* **`String` with any type**: The non-String operand is promoted to a String, and they are joined together (e.g., `"Value: " + 1.5` is `"Value: 1.5"`).

---

## Wrap-Up: Solidifying Your Expression Mental Model

By completing this lab, you have gained vital mastery over how computations are written and executed in Java:
* Expressions evaluate to one value.
* Reassignments overwrite existing variable slots in memory.
* Operator precedence rules can be controlled via parentheses `()`.
* Mixing data types causes automatic conversion (promotion) to the more precise type (`double` or `String`).
