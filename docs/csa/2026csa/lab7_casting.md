---
title: Lab 7 Casting
layout: page
nav_order: 7
parent: 2026csa
math: mathjax
---

# Lab 7: Casting in Java
{: .no_toc }

## Goals
{: .no_toc}
* Understand what **type casting** is and when it is necessary in Java.
* Distinguish between **Widening Casts** (automatic) and **Narrowing Casts** (manual).
* Master the **truncation effect** when converting floating-point numbers to integers.
* Learn and apply the standard AP CSA rounding formulas for both positive and negative numbers.
* Understand **casting operator precedence** and learn to avoid the "too late casting" bug with division.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: What is Casting? (Widening vs. Narrowing)

In programming, we often need to convert a variable or expression from one data type to another. This process is known as **type casting** (Slide 3). 

Java classifies casting into two major categories depending on whether there is a risk of losing information:

### 1. Widening Cast (Automatic)
Widening casting occurs when converting a **smaller** data type to a **larger** data type (specifically, converting an integer type to a floating-point type, such as `int` to `double`). 

Since a `double` can easily represent any integer without any loss of precision, Java performs this conversion **automatically** (Slide 3).

```java
int myInt = 10;
double myDouble = myInt; // Automatic casting: int to double
System.out.println(myDouble); // Prints 10.0
```

### 2. Narrowing Cast (Manual)
Narrowing casting occurs when converting a **larger** data type to a **smaller** data type (such as `double` to `int`). 

Because converting a decimal number to an integer results in losing the fractional part (information loss), Java **will not** perform this automatically. If you try, you will get a compile-time "possible loss of precision" error.

To force Java to convert the value, you must use the **type casting operator**: `(targetType)` (Slide 3).

```java
double myDouble = 9.78;
int myInt = (int) myDouble; // Manual casting: double to int using (int) operator
System.out.println(myInt); // Prints 9
```

---

## Example A: Widening and Narrowing in Action

Easy
{: .label .label-green }

Let's set up a playground to see how compiler rules differ for widening and narrowing casting.

Create a file named `CastingSandbox.java` and try compiling and running the code below:

```java
public class CastingSandbox {
    public static void main(String[] args) {
        // 1. Widening Cast (Automatic)
        int cleanScore = 95;
        double decimalScore = cleanScore; // Automatically becomes 95.0
        System.out.println("Original integer score: " + cleanScore);
        System.out.println("Widened double score: " + decimalScore);

        // 2. Narrowing Cast (Manual)
        double temperature = 98.6;
        // int wholeTemp = temperature; // UNCOMMENT THIS: It will fail to compile!
        int wholeTemp = (int) temperature; // Explicit cast is required
        System.out.println("Original temperature double: " + temperature);
        System.out.println("Narrowed temperature integer: " + wholeTemp);

        // 3. Modifying state check
        // Note: Casting temperature to an int does NOT change the temperature variable itself!
        System.out.println("Original temperature variable is still: " + temperature);
    }
}
```

### Expected Output:
```
Original integer score: 95
Widened double score: 95.0
Original temperature double: 98.6
Narrowed temperature integer: 98
Original temperature variable is still: 98.6
```

---

## Task A: Casting & Truncation Predictor

Easy
{: .label .label-green }

Test your analytical skills on how Java handles type conversions! Below are 5 short questions testing your prediction capabilities. Analyze them carefully.

### Questions:
1. **True or False:** If `double d = 15;` is executed, the statement compiles because Java automatically widens the integer literal `15` to `15.0`.
2. **True or False:** The statement `int x = (int) 5.99;` results in `x` holding the value `6` because Java rounds to the nearest whole number.
3. **True or False:** If you have `int age = 17;` and write `double dAge = (double) age;`, the variable `age` is permanently changed to a `double` type.
4. **Identify the Error:** Why will the following code not compile?
   ```java
   double gasPrice = 3.59;
   int wholePrice = gasPrice;
   ```
5. **Trace the Output:** What does the following code print?
   ```java
   int a = 10;
   double b = 4.5;
   int c = (int) b;
   System.out.println(a + c);
   ```

### Task A Solutions:

<details markdown="1">
<summary><b>Click to reveal Task A Solutions</b></summary>

Create a file named `CastingPredictor.txt` or write your answers inside a comment block at the top of your upcoming programs to verify your answers:

```
Question 1: TRUE (Converting int to double is a widening cast and occurs automatically).
Question 2: FALSE (Casting to an int causes standard truncation, meaning the decimal portion .99 is simply discarded, leaving the value 5).
Question 3: FALSE (Casting creates a temporary value of the new type for the expression; it does not change the declared type or contents of the original variable).
Question 4: The code attempts a narrowing cast (double to int) without an explicit (int) cast operator, which causes a "possible loss of precision" compiler error.
Question 5: Prints 14 (b is 4.5; (int) b truncates to 4; a + c evaluates to 10 + 4, which is 14).
```

</details>

---

## Part 2: Casting Effects & Rounding

### Truncation Effect
As shown in Slide 4, casting a `double` value to an `int` value causes the decimal places to be **truncated** (dropped off entirely), regardless of how close the value is to the next whole number.

* `(int) 7.2` becomes `7`
* `(int) 7.9` becomes `7`
* `(int) -3.8` becomes `-3`

### Rounding Strategy (Slide 5)
If we want to round a `double` to the **nearest integer** (standard half-up rounding, e.g., $$7.5 \rightarrow 8$$, $$7.2 \rightarrow 7$$), we can use the following standard formulas.

#### 1. Rounding Positive Numbers:
To round a positive `double` `x`, we **add `0.5`** to the number before casting it to an `int`:
$$\text{rounded} = \text{(int)}(x + 0.5)$$

* **Trace $$7.2$$:**
  1. Add `0.5`: $$7.2 + 0.5 = 7.7$$
  2. Cast to `int`: `(int) 7.7` truncates to **`7`** (Correct!).
* **Trace $$7.5$$:**
  1. Add `0.5`: $$7.5 + 0.5 = 8.0$$
  2. Cast to `int`: `(int) 8.0` truncates to **`8`** (Correct!).

#### 2. Rounding Negative Numbers:
To round a negative `double` `x` to its nearest integer, we **subtract `0.5`** before casting to an `int`:
$$\text{rounded} = \text{(int)}(x - 0.5)$$

* **Trace $$-9.6$$:**
  1. Subtract `0.5`: $$-9.6 - 0.5 = -10.1$$
  2. Cast to `int`: `(int) -10.1` truncates to **`-10`** (Correct!).
* **Trace $$-9.2$$:**
  1. Subtract `0.5`: $$-9.2 - 0.5 = -9.7$$
  2. Cast to `int`: `(int) -9.7` truncates to **`-9`** (Correct!).

---

## Example B: Rounding Positive and Negative Numbers

Medium
{: .label .label-yellow }

Let's implement a Java program to verify these AP CSA rounding formulas. 

Create a file named `RoundingDemo.java` and run the code below:

```java
public class RoundingDemo {
    public static void main(String[] args) {
        double posValue1 = 7.2;
        double posValue2 = 7.5;
        double negValue1 = -9.2;
        double negValue2 = -9.6;

        // Apply positive rounding formula: (int)(x + 0.5)
        int roundedPos1 = (int) (posValue1 + 0.5);
        int roundedPos2 = (int) (posValue2 + 0.5);

        // Apply negative rounding formula: (int)(x - 0.5)
        int roundedNeg1 = (int) (negValue1 - 0.5);
        int roundedNeg2 = (int) (negValue2 - 0.5);

        System.out.println(posValue1 + " rounded is " + roundedPos1);
        System.out.println(posValue2 + " rounded is " + roundedPos2);
        System.out.println(negValue1 + " rounded is " + roundedNeg1);
        System.out.println(negValue2 + " rounded is " + roundedNeg2);
    }
}
```

### Expected Output:
```
7.2 rounded is 7
7.5 rounded is 8
-9.2 rounded is -9
-9.6 rounded is -10
```

---

## Task B: Temperature Rounder

Medium
{: .label .label-yellow }

Write a Java program named `TempRounder.java` that automatically decides how to round temperature values based on whether they are positive or negative. You will use a selection statement (`if-else`) to dynamically choose the correct rounding formula!

### Instructions:
1. Create a Java file named `TempRounder.java`.
2. Inside `main`, initialize three temperature variables representing different readings:
   * `double temp1 = 32.7;`  *(Positive)*
   * `double temp2 = -4.3;`  *(Negative)*
   * `double temp3 = -15.5;` *(Negative)*
3. Write a block of code for each temperature that rounds it to the nearest integer.
   * Use an `if-else` statement to check if the temperature is greater than or equal to `0.0`.
   * **If positive:** apply `(int)(temp + 0.5)`.
   * **If negative:** apply `(int)(temp - 0.5)`.
4. Print out the original temperature alongside its rounded integer value.

### Expected Output:
```
Original: 32.7 -> Rounded: 33
Original: -4.3 -> Rounded: -4
Original: -15.5 -> Rounded: -16
```

### Code Skeleton:
```java
public class TempRounder {
    public static void main(String[] args) {
        double temp1 = 32.7;
        double temp2 = -4.3;
        double temp3 = -15.5;

        // Process temp1
        int rounded1;
        if (temp1 >= 0) {
            rounded1 = (int) (temp1 + 0.5);
        } else {
            rounded1 = (int) (temp1 - 0.5);
        }
        System.out.println("Original: " + temp1 + " -> Rounded: " + rounded1);

        // TODO 1: Process and print temp2 using if-else

        // TODO 2: Process and print temp3 using if-else
    }
}
```

---

## Part 3: Casting Operator Precedence & Division Pitfalls

### Casting Operator Precedence
As highlighted in Slide 6, **type casting has higher precedence than most arithmetic operators** (like `*`, `/`, `+`, `-`). This leads to two critical scenarios:

### Scenario 1: Casting Before Division (Correct Way)
If you apply the cast operator directly to one of the integer operands *before* dividing, Java converts that operand to a `double` first. Because one operand is now a `double`, Java performs **floating-point division** instead of integer division (Slide 7).

```java
int money = 10;
int people = 3;
double moneyPerPerson = (double) money / people; 
```
* **Step 1:** `(double) money` converts `10` to `10.0`.
* **Step 2:** `10.0 / 3` is evaluated. Since one term is a `double`, Java does floating-point division, yielding `3.3333333333333335`.
* **Step 3:** The result is assigned to `moneyPerPerson`.

### Scenario 2: Arithmetic Before Casting (The "Too Late" Bug)
If you group the division in parentheses and place the cast operator outside, Java evaluates the division *first* as integer division (since both operands are integers), losing the fractional part. The casting happens **too late** (Slide 8).

```java
int money = 10;
int people = 3;
double moneyPerPerson = (double) (money / people); 
```
* **Step 1:** `(money / people)` divides `10` by `3` using integer division, yielding `3`.
* **Step 2:** `(double) 3` converts the integer `3` to `3.0`.
* **Step 3:** The result `3.0` is assigned to `moneyPerPerson`. (Precision is already lost!)

---

## Example C: Avoiding Integer Division Bugs

Hard
{: .label .label-red }

In grading applications or statistics, integer division is a common source of bugs. Look at the difference proper casting makes:

Create a file named `AverageCalculator.java` and run the code below:

```java
public class AverageCalculator {
    public static void main(String[] args) {
        int earnedPoints = 85;
        int totalPoints = 100;

        // Buggy: yields 0.0 because of integer division (85 / 100 = 0)
        double buggyAverage = earnedPoints / totalPoints; 

        // Correct: (double) earnedPoints yields 85.0, then 85.0 / 100 yields 0.85
        double correctAverage = (double) earnedPoints / totalPoints;

        System.out.println("Buggy Average: " + buggyAverage);
        System.out.println("Correct Average: " + correctAverage);
    }
}
```

### Expected Output:
```
Buggy Average: 0.0
Correct Average: 0.85
```

---

## Task C: Simplified Sports Stats

Hard
{: .label .label-red }

Let's help a high school baseball team calculate player statistics. You will write a program that computes a player's **Batting Average** and **Home Run Percentage** using proper casting to avoid the integer division bug.

### Formulas:

$$\text{Batting Average} = \frac{\text{Hits}}{\text{At-Bats}}$$

$$\text{Home Run Percentage} = \frac{\text{Home Runs}}{\text{Hits}} \times 100$$

### Instructions:
1. Create a Java file named `SportsStats.java`.
2. Inside `main`, initialize the following variables:
   * `int atBats = 47;`
   * `int hits = 16;`
   * `int homeRuns = 2;`
3. Calculate the player's **Batting Average** as a `double`.
   * *Critical constraint: You MUST cast at least one of the variables to `double` before division to prevent the integer division bug (which would result in `0.0`).*
4. Calculate the player's **Home Run Percentage** (what percent of their total hits were home runs) as a `double`.
   * *Critical constraint: Again, use proper casting to ensure decimal accuracy.*
5. Print out all original statistics and the computed batting average and home run percentage.

### Expected Output:
```
At-Bats: 47
Hits: 16
Home Runs: 2
Batting Average: 0.3404255319148936
Home Run Percentage: 12.5%
```

### Code Skeleton:
```java
public class SportsStats {
    public static void main(String[] args) {
        int atBats = 47;
        int hits = 16;
        int homeRuns = 2;

        // TODO 1: Calculate Batting Average (double) using casting
        double battingAverage = 0.0; // Replace with code

        // TODO 2: Calculate Home Run Percentage (double) using casting
        double homeRunPercent = 0.0; // Replace with code

        // Print outputs
        System.out.println("At-Bats: " + atBats);
        System.out.println("Hits: " + hits);
        System.out.println("Home Runs: " + homeRuns);
        System.out.println("Batting Average: " + battingAverage);
        System.out.println("Home Run Percentage: " + homeRunPercent + "%");
    }
}
```

---

## Wrap-Up & Summary

By completing this lab, you have learned:
1. **Widening and Narrowing:** Java automatically widens small types to larger ones (like `int` to `double`), but narrowing (like `double` to `int`) requires the manual `(int)` casting operator.
2. **Truncation Behavior:** Casting a `double` to an `int` always truncates the decimal portion.
3. **Rounding Formulas:** 
   * Positive numbers: `(int)(x + 0.5)`
   * Negative numbers: `(int)(x - 0.5)`
4. **Casting Precedence:** Casting binds tighter than standard arithmetic. To avoid the "too late" division bug, cast the numerator or denominator individually *before* division.
