---
title: Lab 5 Output and Constant
layout: page
nav_order: 5
parent: 2026csa
math: mathjax
---

# Lab 5: Output, Escape Sequences, and Constants
{: .no_toc }

## Goals
{: .no_toc}
* Understand the difference between `System.out.print()` and `System.out.println()`.
* Master **String evaluation order** during concatenation (`+`) with other types.
* Learn what **literals** are and how to use them.
* Learn and utilize key **escape sequences** (`\n`, `\t`, `\\`, `\"`, `\'`) to format console output.
* Master **symbolic constants** using the `final` keyword, and understand the core benefits of using constants (clarity, safety, maintenance, flexibility).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Printing Output and String Evaluation Order

To see the results of our computations, we must display them on the screen (Slide 3). In Java, we use two primary output methods:

* **`System.out.println()`**: Prints the specified text and then **moves the cursor to the next line**.
* **`System.out.print()`**: Prints the specified text and **leaves the cursor on the same line**.

### String Concatenation Evaluation Order (Slide 5)
When you combine Strings and numbers with the `+` operator, Java evaluates the expression **from left to right**. 

Because of this order, look at how the same operators produce completely different results:

1. **`"Score: " + 10 + 20`**
   * Left-to-right evaluation: first, `"Score: " + 10` is evaluated $$\rightarrow$$ `"Score: 10"` (a String).
   * Next, `"Score: 10" + 20` is evaluated $$\rightarrow$$ `"Score: 1020"`.
2. **`10 + 20 + " Score"`**
   * Left-to-right evaluation: first, `10 + 20` is evaluated $$\rightarrow$$ `30` (numerical addition).
   * Next, `30 + " Score"` is evaluated $$\rightarrow$$ `"30 Score"`.

---

## Example A: print vs. println and Order of Concatenation

Easy
{: .label .label-green }

Let's build a small program to observe how cursor movements and concatenation evaluation order work in practice.

Create a file named `OutputPlayground.java` in IntelliJ and run the code below:

```java
public class OutputPlayground {
    public static void main(String[] args) {
        // 1. Using print vs println
        System.out.print("Hello ");
        System.out.print("World!"); 
        System.out.println(); // Just moves the cursor to a new line

        System.out.println("Line 1");
        System.out.println("Line 2");

        // 2. Observing order of evaluation
        System.out.println("Result: " + 5 + 5); // Prints "Result: 55"
        System.out.println(5 + 5 + " :Result"); // Prints "10 :Result"
    }
}
```

### Expected Output:
```
Hello World!
Line 1
Line 2
Result: 55
10 :Result
```

---

## Task A: Scoreboard Aligner

Easy
{: .label .label-green }

Write a Java program named `Scoreboard.java` that displays a retro arcade scoreboard. You will practice using both `print` and `println` to align outputs on a single line, and write mixed-type output statements that evaluate correctly without losing arithmetic operations.

### Instructions:
1. Create a Java file named `Scoreboard.java`.
2. Inside `main`, initialize the following variables:
   * `String teamA = "Knights";`
   * `String teamB = "Dragons";`
   * `int scoreA1 = 150;`
   * `int scoreA2 = 75;`
   * `int scoreB1 = 120;`
   * `int scoreB2 = 90;`
3. Use **only** `System.out.print()` to print `"Knights Total Score: "` and the sum of `scoreA1` and `scoreA2` on the **same line**.
   * *Hint: To perform numeric addition before String concatenation, you must wrap the math inside parentheses, e.g., `(scoreA1 + scoreA2)`.*
4. Use `System.out.println()` to display `"Dragons Total Score: "` and the sum of `scoreB1` and `scoreB2` on the next line.
5. Make sure the output formats exactly match the expected output below!

### Expected Output:
```
Knights Total Score: 225
Dragons Total Score: 210
```

### Code Skeleton:
```java
public class Scoreboard {
    public static void main(String[] args) {
        String teamA = "Knights";
        String teamB = "Dragons";
        int scoreA1 = 150;
        int scoreA2 = 75;
        int scoreB1 = 120;
        int scoreB2 = 90;

        // TODO 1: Print Knights' sum score on one line using print() and parentheses

        // TODO 2: Print Dragons' sum score on the next line using println() and parentheses
    }
}
```

---

## Part 2: Literals and Escape Sequences

In programming, a **literal** is the source code representation of a fixed value (Slide 7).
* `42` is an integer literal.
* `3.14` is a double literal.
* `'A'` is a character literal.
* `"hello"` is a **String literal** (a sequence of characters enclosed in double quotes) (Slide 8).

### Escape Sequences (Slide 9)
Because some characters have special meanings in Java (like the double quote `"` which defines the beginning and end of a String), we must use **escape sequences** to print them safely. An escape sequence starts with a backslash `\` followed by another character:

| Escape Sequence | Name | Meaning |
| :--- | :--- | :--- |
| `\n` | Newline | Inserts a line break (starts a new line) |
| `\t` | Tab | Inserts a horizontal tab space (for alignment) |
| `\\` | Backslash | Prints a literal backslash `\` character |
| `\"` | Double Quote | Prints a literal double quote `"` inside a String |
| `\'` | Single Quote | Prints a literal single quote `'` |

---

## Example B: Escape Sequences in Action

Easy
{: .label .label-green }

Let's build a program to display structured lines, including tabs, newlines, and quotes (Slide 10).

Create a file named `LyricPrinter.java` in IntelliJ and run the code below:

```java
public class LyricPrinter {
    public static void main(String[] args) {
        // Outputting newline, tab, and double quotes
        System.out.println("\nLet it go,\n\tlet it go");

        // Outputting backslashes and single quotes
        System.out.println("Java folder: C:\\Program Files\\Java\\");
        System.out.println("The teacher said, \"Always finish your homework!\"");
    }
}
```

### Expected Output:
```

Let it go,
	let it go
Java folder: C:\Program Files\Java\
The teacher said, "Always finish your homework!"
```

---

## Task B: Quote and Receipt Formatter

Easy
{: .label .label-green }

Write a Java program named `ReceiptFormatter.java` that prints a beautifully aligned customer receipt using tab characters, newlines, and double quotes.

### Instructions:
1. Create a class named `ReceiptFormatter` in a file named `ReceiptFormatter.java`.
2. Your program should display the following layout exactly. It must contain:
   * A title header in double quotes: `"CS ACADEMY BAKERY"` (include the literal double quotes).
   * A blank line (using a newline character `\n`).
   * A tabbed items list aligning the names and prices:
     * `Donut    $2.50`
     * `Coffee   $3.00`
   * Another blank line.
   * A final message with a backslash: `Have a nice day! (Find us at: www.cs.academy/bakery)`.
3. Try to do this using as few print statements as possible by embedding `\n` and `\t` inside your String literals!

### Expected Output:
```
"CS ACADEMY BAKERY"

Item		Price
Donut		$2.50
Coffee		$3.00

Have a nice day! (Find us at: www.cs.academy\bakery)
```

### Code Skeleton:
```java
public class ReceiptFormatter {
    public static void main(String[] args) {
        // TODO: Print the formatted receipt using escape sequences (\n, \t, \", \\)
        // Ensure you print the literal quotes around "CS ACADEMY BAKERY" 
        // and the backslash in the URL.
    }
}
```

---

## Part 3: Symbolic Constants and the `final` Keyword

A **symbolic constant** is an initialized variable whose value cannot be changed once assigned (Slide 12). In Java, we declare constants using the `final` keyword.

### Format and Naming Conventions
```java
final double PI = 3.14159;
final int MAX_PLAYERS = 4;
```
* **Syntax**: Always put `final` before the data type.
* **Style**: In AP CSA, constants are traditionally written in **UPPERCASE** with underscores separating words (e.g., `SALES_TAX_RATE`). This makes them immediately distinguishable from standard variables!

---

## Why Use Symbolic Constants? (Slide 13)

Using initialized `final` variables instead of typing raw numbers (known as "magic numbers") provides four critical benefits:

1. ⚙️ **Global Maintenance**: If you use a constant like `TAX_RATE = 0.08;` twenty times in your code, and the tax rate changes to `0.09`, you only have to change it **once** at the declaration line.
2. 🔀 **Flexibility**: If a constant value later needs to vary depending on user actions, you can easily turn it back into a standard variable by just removing the `final` keyword.
3. 📖 **Code Clarity**: Raw numbers like `299792458` are obscure. Writing `final int SPEED_OF_LIGHT = 299792458;` makes your expressions self-documenting and easy to read.
4. 🛡️ **Compiler Safety**: The compiler actively blocks anyone from accidentally modifying a constant. If some part of your code tries to run `PI = 3.0;`, the compiler will refuse to build and flag a syntax error, preventing bugs!

---

## Example C: Constants and Compiler Safety

Easy
{: .label .label-green }

Let's test what happens when we attempt to modify a `final` constant in Java.

Create a file named `ConstantTest.java` in IntelliJ:

```java
public class ConstantTest {
    public static void main(String[] args) {
        final double METERS_TO_FEET = 3.28084;
        System.out.println("1 meter is " + METERS_TO_FEET + " feet.");

        // Uncommenting the line below will trigger a compiler compile-time error!
        // METERS_TO_FEET = 3.0;
    }
}
```

If you uncomment `METERS_TO_FEET = 3.0;`, the compiler will report:
`error: cannot assign a value to final variable METERS_TO_FEET`

---

## Task C: Physics Calculator

Easy
{: .label .label-green }

In physics, several quantities remain absolutely constant. Write a program named `PhysicsCalculator.java` that calculates the distance sound travels and the weight of an object on Earth using symbolic constants.

### Instructions:
1. Create a class `PhysicsCalculator` in a file named `PhysicsCalculator.java`.
2. Declare two symbolic constants inside your `main` method using the `final` keyword:
   * `SPEED_OF_SOUND` set to `343.0` (in meters per second).
   * `EARTH_GRAVITY` set to `9.8` (acceleration in $$m/s^2$$).
3. Declare two standard variables:
   * `secondsElapsed = 5.0` (representing travel time).
   * `objectMass = 12.0` (representing mass in kilograms).
4. Perform the calculations:
   * Calculate `distanceTraveled` by multiplying `SPEED_OF_SOUND` by `secondsElapsed`.
   * Calculate `weight` by multiplying `objectMass` by `EARTH_GRAVITY`.
5. Print the distance traveled (in meters) and weight (in Newtons) exactly as shown in the expected output below.
6. **Try the safety test**: Add a line `EARTH_GRAVITY = 1.6;` at the end of your program and compile. Verify that the compiler blocks it, then comment out or delete that line before finalizing your program.

### Expected Output:
```
In 5.0 seconds, sound travels 1715.0 meters.
A mass of 12.0 kg weighs 117.6 Newtons.
```

### Code Skeleton:
```java
public class PhysicsCalculator {
    public static void main(String[] args) {
        // TODO 1: Declare symbolic constants SPEED_OF_SOUND and EARTH_GRAVITY

        // TODO 2: Initialize secondsElapsed (5.0) and objectMass (12.0)

        // TODO 3: Compute distance and weight
        double distance = 0.0; // Update this
        double weight = 0.0;   // Update this

        // TODO 4: Print results in specified format

        // TODO 5: Safety test - try assigning a new value to EARTH_GRAVITY and compile!
    }
}
```

---

## AP CSA Output, Escape Sequences, and Constants Cheatsheet

Here is your quick-reference summary of outputs, escape codes, and constants tested on the AP Computer Science A exam.

### Output Methods
* `System.out.print(x)`: Outputs `x` and keeps the cursor on the same line.
* `System.out.println(x)`: Outputs `x` and moves the cursor to the beginning of the next line.

### Order of Concatenation
* Concatenations are evaluated strictly **left-to-right**.
* Any number added to a String becomes a String.
* `1 + 2 + "x"` evaluates to `"3x"`.
* `"x" + 1 + 2` evaluates to `"x12"`.

### Escape Sequences
* `\n`: Starts a new line.
* `\t`: Inserts a horizontal space (aligns items).
* `\\`: Represents a single backslash character.
* `\"`: Inserts a double quote inside a String literal.

### Symbolic Constants
* Declared using the `final` keyword (e.g., `final int SEATS = 50;`).
* A `final` variable can only be initialized once and **never reassigned**.
* Writing constants in **UPPERCASE_WITH_UNDERSCORES** is standard Java style.
* Benefits:
  1. **Clarity**: Explains "magic numbers" in code.
  2. **Compiler Safety**: Prevents accidental re-assignments.
  3. **Global Maintenance**: Changes values in one single place.
  4. **Flexibility**: Easily turns back into standard variables.

---

## Wrap-Up: Building Production-Ready Habits

By completing this lab, you have gained vital software engineering discipline:
* You understand how text cursor positions translate to formatted lines on a console.
* You know how to safely use escape sequences to bypass String formatting restrictions.
* You understand the structural importance of symbolic constants (`final` variables) for building self-documenting, maintainable, and bug-resistant applications.
