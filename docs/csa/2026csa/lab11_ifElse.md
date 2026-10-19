---
title: Lab 11 Selection & If-Else
layout: page
nav_order: 11
parent: 2026csa
math: mathjax
---

# Lab 11: Selection & Control Flow (if / else)
{: .no_toc }

## Goals
{: .no_toc}
* Understand conditional execution flow using `if`, `if-else`, and `else-if` chains.
* Master **boolean expressions** using relational operators (`==`, `!=`, `<`, `<=`, `>`, `>=`) and logical operators (`&&`, `||`, `!`).
* Memorize and apply Java's **Operator Precedence (Rank of Operators)**.
* Apply **De Morgan’s Laws** to negate and simplify complex boolean expressions.
* Distinguish reference equality (`==` and `!=`) from value equality (`.equals()` and `.equalsIgnoreCase()`) for reference types like `String`.
* Internalize **short-circuit evaluation** with `&&` and `||` to construct safe guard conditions.
* Avoid the classic **dangling-else ambiguity** by always enclosing control blocks in curly braces `{ }`.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Boolean Expressions & Relational Operators (Slide 2–5)

A **boolean expression** is any Java expression that evaluates to either `true` or `false` (Slide 2).

### 1. Relational Operators (Slide 5)
Relational operators compare primitive values and return a `boolean`:

| Operator | Meaning | Evaluates to `true` | Evaluates to `false` |
| :---: | :--- | :--- | :--- |
| `==` | Equal to | `2 == 2` | `2 == 3` |
| `!=` | Not equal to | `2 != 3` | `2 != 2` |
| `<` | Strictly less than | `2 < 3` | `2 < 2` |
| `<=` | Less than or equal to | `2 <= 2` | `2 <= -1` |
| `>` | Strictly greater than | `13 > 2` | `2 > 13` |
| `>=` | Greater than or equal to | `3 >= 2` | `2 >= 13` |

### 2. Logical Operators (Slide 3, 4)
Logical operators connect and modify boolean expressions:

* **Logical AND (`&&`)**: `a && b` is `true` if and only if **both** `a` and `b` are `true`, and `false` otherwise.
* **Logical OR (`||`)**: `a || b` is `true` if **either** `a` or `b` (or both) are `true`, and `false` otherwise.
* **Logical NOT (`!`)**: `!a` inverts the truth value (`true` becomes `false`, `false` becomes `true`).

#### Truth Tables (Slide 4)

| `a` | `!a` |
| :---: | :---: |
| `true` | `false` |
| `false` | `true` |

| `a` | `b` | `a && b` | `a || b` |
| :---: | :---: | :---: | :---: |
| `false` | `false` | `false` | `false` |
| `false` | `true` | `false` | `true` |
| `true` | `false` | `false` | `true` |
| `true` | `true` | `true` | `true` |

---

## Part 2: Rank of Operators & Precedence (Slide 6)

When expressions combine arithmetic, comparison, and logical operators, Java resolves them based on operator precedence from **HIGH** to **LOW** (Slide 6):

```
       HIGH  ▲   1. Multiplicative :  *   /   %
             │   2. Additive       :  +   -
             │   3. Relational     :  <   <=  >   >=
             │   4. Equality       :  ==  !=
             │   5. Logical AND    :  &&
       LOW   ▼   6. Logical OR     :  ||
```

### Precedence Example Trace:
```java
int x = 5;
int y = 10;
boolean result = x + 2 * 3 > y && y - 1 == 9;
```
1. **Multiplicative (`*`)**: `2 * 3` $$\rightarrow$$ `6`
2. **Additive (`+`, `-`)**: `x + 6` $$\rightarrow$$ `11`, and `y - 1` $$\rightarrow$$ `9`
3. **Relational (`>`)**: `11 > y` (`11 > 10`) $$\rightarrow$$ `true`
4. **Equality (`==`)**: `9 == 9` $$\rightarrow$$ `true`
5. **Logical AND (`&&`)**: `true && true` $$\rightarrow$$ `true`

---

## Part 3: De Morgan’s Laws (Slide 7)

**De Morgan's Laws** describe how to distribute a negation (`!`) across a compound boolean expression:

$$\neg (a \land b) \iff \neg a \lor \neg b \quad \implies \quad \texttt{!(a \&\& b)} \equiv \texttt{!a || !b}$$

$$\neg (a \lor b) \iff \neg a \land \neg b \quad \implies \quad \texttt{!(a || b)} \equiv \texttt{!a \&\& !b}$$

### Concrete Numerical Examples:

1. **Negating an AND condition:**
   ```java
   // Original: x is within [0, 2] inclusive
   0 <= x && x <= 2

   // Negated with !:
   !(0 <= x && x <= 2)

   // Applying De Morgan's Law: flip && to || and invert both comparisons:
   x < 0 || x > 2
   ```

2. **Negating an OR condition:**
   ```java
   // Original: x is strictly outside the range [0, 2]
   x < 0 || x > 2

   // Negated with !:
   !(x < 0 || x > 2)

   // Applying De Morgan's Law: flip || to && and invert both comparisons:
   x >= 0 && x <= 2
   ```

---

## Part 4: Comparing Objects: `==` vs. `.equals()` (Slide 8–10)

{: .warning }
> **Crucial AP CSA Rule:**
> * `==` and `!=` compare **memory references (addresses)**, NOT values or contents! (Slide 8).
> * To compare the contents of two `String` objects, **always** use `.equals()` or `.equalsIgnoreCase()` (Slide 10).

```java
// DON'T DO THIS:
String cmd = console.readLine();
if (cmd == "Help") { ... } // BUG! Compares memory references, usually evaluates to false!

// DO THIS INSTEAD:
if (cmd.equals("Help")) { ... } // Correct! Compares the actual character sequences.
```

### When to use `==` and `!=` with Objects (Slide 9):
1. **Comparing against `null`:** To check if an object reference actually points to an allocated instance in memory:
   ```java
   String text = file.readLine();
   if (text != null) {
       System.out.println("Line length: " + text.length());
   }
   ```
2. **Checking identity:** When you specifically want to test if two references point to the exact same memory address.

---

## Part 5: The `if` Statement & Anatomy (Slide 11–13)

Selection statements allow your code to execute specific instructions depending on the runtime values of variables (Slide 12).

### Anatomy of an `if` Statement (Slide 13):
```java
if (boolean_expression) {
    // Statement block: enclosed in { }, executes sequentially if true
    statement1;
    statement2;
}
```

```
          [ Evaluate boolean_expression ]
                    /          \
              true /            \ false
                  ▼              │
         [ Execute Block ]       │
                  │              │
                  ▼              ▼
           [ Continue Sequential Execution ]
```

---

## Example A: Sorting Two Variables (`TwoSort`) (Slide 14)

Easy
{: .label .label-green }

Suppose we want to output two numbers `a` and `b` in non-decreasing order. If `a > b`, we swap their values using a temporary variable `t`.

```java
import java.util.Scanner;

public class TwoSort {
    public static void main(String[] args) {
        System.out.println("Please enter 2 ints");
        Scanner scanner = new Scanner(System.in);
        int a = scanner.nextInt();
        int b = scanner.nextInt();

        // Swap if out of order
        if (a > b) {
            int t = a;
            a = b;
            b = t;
        }

        System.out.println(a);
        System.out.println(b);
    }
}
```

### Sample Run:
```
Please enter 2 ints
32 23
23
32
```

---

## Task A: Implement & Test `TwoSort` (Slide 14)

Easy
{: .label .label-green }

### Instructions:
1. Create a file named `TwoSort.java`.
2. Use `Scanner` to prompt the user and read two integers `a` and `b`.
3. If `a > b`, use a temporary variable `int t` to swap them so that `a` holds the smaller value and `b` holds the larger value.
4. Print `a` and `b` on separate lines.

### Expected Output:
If input is `32 23`:
```
Please enter 2 ints
23
32
```
If input is `15 40`:
```
Please enter 2 ints
15
40
```

---

## Task B: Maximum of Three Numbers (`three max`) (Slide 15)

Medium
{: .label .label-yellow }

Write a method that returns the largest of three integers using conditional statements.

### Instructions:
1. Create a file named `ThreeMax.java`.
2. Implement the static method:
   ```java
   public static int max(int x, int y, int z)
   ```
3. Initialize a variable `maximum = x`. If `y > maximum`, update `maximum = y`. If `z > maximum`, update `maximum = z`. Finally, return `maximum`. (Alternatively, use compound boolean expressions).
4. Test your method with various parameter orders in `main()`.

### Code Skeleton:
```java
public class ThreeMax {
    // Complete the method signature to return the maximum of x, y, and z (Slide 15)
    public static int max(int x, int y, int z) {
        // TODO: Determine and return the largest of x, y, and z
        return 0; // Update this
    }

    public static void main(String[] args) {
        System.out.println("max(3, 7, 5)  -> " + max(3, 7, 5));     // 7
        System.out.println("max(10, 2, 4) -> " + max(10, 2, 4));   // 10
        System.out.println("max(1, 8, 9)  -> " + max(1, 8, 9));     // 9
        System.out.println("max(6, 6, 6)  -> " + max(6, 6, 6));     // 6
    }
}
```

### Expected Output:
```
max(3, 7, 5)  -> 7
max(10, 2, 4) -> 10
max(1, 8, 9)  -> 9
max(6, 6, 6)  -> 6
```

---

## Part 6: Short-Circuit Evaluation (Slide 16)

Java evaluates compound boolean expressions from left to right using **short-circuit evaluation**:
* `condition1 && condition2`: If `condition1` is `false`, Java **skips** evaluating `condition2` because the entire expression is already known to be `false`.
* `condition1 || condition2`: If `condition1` is `true`, Java **skips** evaluating `condition2` because the entire expression is already known to be `true`.

### Guard Conditions:
Short-circuit evaluation is essential for writing **guard conditions** that prevent fatal runtime exceptions:

```java
// Safe against taking the square root of a negative number (Slide 16):
if (x >= 0 && Math.sqrt(x) < 15.0) {
    System.out.println("Valid non-negative value with sqrt < 15");
}

// Safe against ArithmeticException (Division by zero):
if (denom != 0 && num / denom > 2) {
    System.out.println("Safe division!");
}

// Safe against NullPointerException:
if (str != null && str.length() > 0) {
    System.out.println("Safe string inspection: " + str);
}
```

---

## Part 7: The `if-else` Statement (Slide 17, 18)

When you need to choose between two alternative execution paths, use an `if-else` statement (Slide 17):

```java
if (boolean_expression) {
    // executes if boolean_expression is true
} else {
    // executes if boolean_expression is false
}
```

### Example B: Coin Flip Simulation (Slide 18)
`Math.random()` returns a pseudorandom `double` in $$[0.0, 1.0)$$. We can simulate a fair coin flip:

```java
public class Flip {
    public static void main(String[] args) {
        if (Math.random() < 0.5) {
            System.out.println("Heads");
        } else {
            System.out.println("Tails");
        }
    }
}
```

---

## Task C: Absolute Value Method (`abs`) (Slide 19)

Easy
{: .label .label-green }

In mathematics, the absolute value $$\|n\|$$ is defined as:
* $$\|n\| = n$$ if $$n \ge 0$$
* $$\|n\| = -n$$ if $$n < 0$$

### Instructions:
1. Create a file named `AbsoluteValue.java`.
2. Implement the static method:
   ```java
   public static int abs(int n)
   ```
3. Use an `if` or `if-else` statement to return the absolute value of `n`.
4. Test with negative, positive, and zero values.

### Code Skeleton:
```java
public class AbsoluteValue {
    // Complete the method signature to return the absolute value of n (Slide 19)
    public static int abs(int n) {
        // TODO: Return n if positive/zero, or -n if negative
        return 0; // Update this
    }

    public static void main(String[] args) {
        System.out.println("abs(-15): " + abs(-15)); // 15
        System.out.println("abs(27):  " + abs(27));  // 27
        System.out.println("abs(0):   " + abs(0));   // 0
    }
}
```

### Expected Output:
```
abs(-15): 15
abs(27):  27
abs(0):   0
```

---

## Part 8: Nested `if` Statements & Multi-Way Branching (Slide 20–22)

### 1. `if-else if-else` Multi-Way Chains (Slide 20)
When evaluating multiple mutually exclusive conditions, use an `if-else if-else` chain:

```java
// Grade Classification (Slide 20)
if (grade >= 90) {
    letter = 'A';
} else if (grade >= 80) {
    letter = 'B';
} else if (grade >= 70) {
    letter = 'C';
} else {
    letter = 'F';
}
```

{: .note }
Java tests conditions in order from top to bottom and exits the entire chain as soon as one condition evaluates to `true`.

---

## Task D: Ticket Price Calculator (`getTicketPrice`) (Slide 21)

Medium
{: .label .label-yellow }

Practice hierarchical decision-making using **nested `if` statements** (Slide 21).

### Pricing Rules:
* If `age < 12`: Ticket price is `"$5"`.
* If `age >= 12`:
  * If `isStudent` is `true`: Ticket price is `"$8"`.
  * If `isStudent` is `false`: Ticket price is `"$12"`.

### Instructions:
1. Create a file named `NestedIfExercise.java`.
2. Implement the method:
   ```java
   public static String getTicketPrice(int age, boolean isStudent)
   ```
3. Use a nested `if` structure to evaluate the age and student status.
4. Test all branches in `main()`.

### Code Skeleton:
```java
public class NestedIfExercise {
    public static String getTicketPrice(int age, boolean isStudent) {
        // Complete the nested if statement to calculate ticket price (Slide 21):
        // - Age < 12: "$5"
        // - Age >= 12: if isStudent is true -> "$8", else -> "$12"
        return ""; // Update this
    }

    public static void main(String[] args) {
        System.out.println("Age 8, student=false:  " + getTicketPrice(8, false));  // "$5"
        System.out.println("Age 16, student=true:  " + getTicketPrice(16, true));  // "$8"
        System.out.println("Age 25, student=false: " + getTicketPrice(25, false)); // "$12"
        System.out.println("Age 12, student=true:  " + getTicketPrice(12, true));  // "$8"
    }
}
```

### Expected Output:
```
Age 8, student=false:  $5
Age 16, student=true:  $8
Age 25, student=false: $12
Age 12, student=true:  $8
```

---

## Part 9: Ambiguity in Nested `if` Statements: The Dangling-Else Problem (Slide 22)

Consider the un-braced code snippet below:

```java
if (expr1)
    if (expr2)
        statementA;
else
    statementB;
```

**Which `if` does the `else` belong to?**

Despite misleading indentation, Java's language specification dictates:
> An `else` clause always pairs with the **nearest preceding unmatched `if`**.

In Java, the code above is strictly equivalent to:

```java
if (expr1) {
    if (expr2) {
        statementA;
    } else {
        statementB; // Matches expr2, NOT expr1!
    }
}
```

If you intended `statementB` to execute when `expr1` is `false`, omitting curly braces causes a silent logic bug!

### The Golden Rule:
Always enclose control bodies in curly braces `{ }`, even when the body contains only one statement. Braces make programmer intent unambiguous, prevent dangling-else defects, and ensure clean, maintainable code.
