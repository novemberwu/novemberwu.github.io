---
title: Unit 3 Lab 3 - Instance Methods & Calculations
layout: page
nav_order: 15
parent: 2026csa
math: mathjax
---

# Unit 3 Lab 3: Instance Methods & State Mutation
{: .no_toc }

## Goals
{: .no_toc }
* Understand the role of **instance methods**: giving objects behaviors, calculations, and mutations.
* Master instance method syntax: `public returnType methodName(parameterList) { ... }`.
* Distinguish between **mutator methods** (modifies internal object state) and **pure methods** (computes and returns a result without altering state).
* Master arithmetic operations between objects that instantiate and return a **new object** of the same class (e.g., `Fraction.add`, `ComplexNum.multiply`).
* Distinguish between `this` (the calling object before the dot: `c1.add(...)`) and parameter objects (`other`).
* Implement defensive guard conditions and handle numeric edge cases (e.g. division by zero, double casting).
* Complete `ComplexNum.java` with subtraction and division.
* Complete `StepTracker.java` with `addDailySteps`, `activeDays`, and `averageSteps` (AP CSA 2019 FRQ 1).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Concepts & Method Design

In Labs 1 and 2, you built the structure of a class: instance variables (state) and constructors (initialization). In Lab 3, you give your objects **behavior**.

```
Calling Syntax:
                 calling object
                      |
                      v
                     c1 . multiply ( c2 );
                                     ^
                                     |
                              parameter object
```

### 1. Anatomy of an Instance Method
An instance method is declared inside the class body without the `static` keyword:

```java
public Fraction add(Fraction other) {
    // method body
}
```

* `public`: Accessible to code outside the class.
* `Fraction` (Return Type): The type of data returned to the caller (`int`, `double`, `boolean`, `String`, an object type, or `void` if nothing is returned).
* `add` (Method Name): By convention, **lowerCamelCase** verb or action phrase.
* `(Fraction other)` (Parameter List): Data passed into the method to assist its computation.

### 2. The Calling Object (`this`) vs. The Parameter Object (`other`)
When you invoke an instance method on an object:
```java
ComplexNum result = num1.add(num2);
```
Inside the `add` method:
* `this.realPart` (or simply `realPart`) refers to the data of the **calling object** (`num1`).
* `other.realPart` refers to the data of the **parameter object** (`num2`).

{: .important }
> In Java, private instance variables of another object of the **same class** are directly accessible inside that class! Within `ComplexNum`, `num1` can directly access `other.realPart` and `other.imaginaryPart`.

### 3. Pure Methods vs. Mutator Methods

| Type | Return Type | Modifies Object State? | Example |
| :--- | :---: | :---: | :--- |
| **Pure / Accessor** | Object / Primitive | **No** (returns new value or object) | `c1.add(c2)`, `f.getValue()` |
| **Mutator** | `void` | **Yes** (updates instance variables) | `tracker.addDailySteps(8000)` |

---

## Part 2: Worked Examples from the Codebase

Let's inspect how instance methods are implemented in `Fraction.java` and `ComplexNum.java` in `~/src/csa/ch3_1`.

### Worked Example 1: `Fraction.java` Arithmetic Methods

```java
package ch3_1;

public class Fraction {
    private int num;
    private int denom;

    // -------------------------------------------------------------
    // Addition: a/b + c/d = (ad + bc) / bd
    // Notice: Neither 'this' nor 'other' is modified!
    // A brand-new Fraction object is created and returned.
    // -------------------------------------------------------------
    public Fraction add(Fraction other) {
        int newNum = this.num * other.denom + this.denom * other.num;
        int newDenom = this.denom * other.denom;
        return new Fraction(newNum, newDenom);
    }

    // -------------------------------------------------------------
    // Multiplication: a/b * c/d = (ac) / (bd)
    // -------------------------------------------------------------
    public Fraction multiply(Fraction other) {
        int newNum = this.num * other.num;
        int newDenom = this.denom * other.denom;
        return new Fraction(newNum, newDenom);
    }

    // -------------------------------------------------------------
    // Value Calculation: Note the explicit double cast!
    // -------------------------------------------------------------
    public double getValue() {
        // Without (double), 1 / 2 would evaluate to 0 due to integer division!
        return (double) this.num / (double) this.denom;
    }
}
```

#### Client Verification (`TestFractions.java`)
```java
Fraction f1 = new Fraction(1, 2);
Fraction f2 = new Fraction(1, 3);

Fraction sum = f1.add(f2);           // 1/2 + 1/3 = 5/6
Fraction product = f1.multiply(f2);  // 1/2 * 1/3 = 1/6

System.out.println(f1 + " + " + f2 + " = " + sum);
System.out.println(f1 + " * " + f2 + " = " + product);
```

---

### Worked Example 2: `ComplexNum.java` Add & Multiply

Now inspect the reference implementation in `~/src/csa/ch3_1/ComplexNum.java`:

```java
package ch3_1;

public class ComplexNum {
    private double realPart;
    private double imaginaryPart;

    // -------------------------------------------------------------
    // Addition: (a + bi) + (c + di) = (a + c) + (b + d)i
    // -------------------------------------------------------------
    public ComplexNum add(ComplexNum other) {
        return new ComplexNum(
            this.realPart + other.realPart, 
            this.imaginaryPart + other.imaginaryPart
        );
    }

    // -------------------------------------------------------------
    // Multiplication: (a + bi)(c + di) = (ac - bd) + (ad + bc)i
    // -------------------------------------------------------------
    public ComplexNum multiply(ComplexNum other) {
        double a = this.realPart;
        double b = this.imaginaryPart;
        double c = other.realPart;
        double d = other.imaginaryPart;

        double newReal = a * c - b * d;
        double newImag = a * d + b * c;

        return new ComplexNum(newReal, newImag);
    }

    // -------------------------------------------------------------
    // Conjugate: z = a + bi  ==>  z* = a - bi
    // -------------------------------------------------------------
    public ComplexNum getConjugate() {
        if (Math.abs(imaginaryPart) > 1e-6) {
            return new ComplexNum(realPart, -1 * imaginaryPart);
        } else {
            return new ComplexNum(realPart);
        }
    }
}
```

---

## Part 3: Student Tasks

Now it's your turn to complete the missing instance methods in `ComplexNum.java` and `StepTracker.java`!

---

### Task 1: Complete Subtraction and Division in `ComplexNum.java`
Medium
{: .label .label-yellow }

Open `ComplexNum.java` from Lab 2. Implement the two remaining arithmetic operations:

#### 1. Subtraction (`substract`)
Subtracting two complex numbers is component-wise:
$$(a + bi) - (c + di) = (a - c) + (b - d)i$$

* **Signature:** `public ComplexNum substract(ComplexNum other)`
* **Return Value:** A new `ComplexNum` whose real part is `this.realPart - other.realPart` and whose imaginary part is `this.imaginaryPart - other.imaginaryPart`.

#### 2. Division (`divide`)
Dividing two complex numbers requires multiplying the numerator and denominator by the complex conjugate of the denominator:
$$\frac{a + bi}{c + di} = \frac{(a + bi)(c - di)}{(c + di)(c - di)} = \frac{(ac + bd) + (bc - ad)i}{c^2 + d^2}$$

* **Signature:** `public ComplexNum divide(ComplexNum other)`
* **Denominator Check:** First calculate $$\text{denominator} = c^2 + d^2 = c \cdot c + d \cdot d$$.
* **Defensive Guard:** If the denominator is zero (or very close to zero: `Math.abs(denominator) < 1e-9`), throw an `ArithmeticException("Cannot divide by 0 + 0i")`.
* **Calculation:**
  $$\text{newReal} = \frac{ac + bd}{c^2 + d^2}, \quad \text{newImag} = \frac{bc - ad}{c^2 + d^2}$$
* **Return Value:** Return `new ComplexNum(newReal, newImag)`.

#### Starter Code Skeleton
```java
// Inside ComplexNum.java:

// TODO 1: Implement subtraction
// Signature: public ComplexNum substract(ComplexNum other)
// - Formula: (a + bi) - (c + di) = (a - c) + (b - d)i
// - Return a new ComplexNum instance


// TODO 2: Implement division
// Signature: public ComplexNum divide(ComplexNum other)
// - Calculate denominator: c * c + d * d
// - Guard: if denominator is zero (Math.abs(denominator) < 1e-9), throw ArithmeticException
// - Calculate newReal and newImag using the conjugate division formula
// - Return a new ComplexNum instance
```

#### Client Verification (`ComplexNumClient.java`)
```java
package ch3_1;

public class ComplexNumClient {
    public static void main(String[] args) {
        ComplexNum z1 = new ComplexNum(4.0, 2.0);
        ComplexNum z2 = new ComplexNum(3.0, -1.0);

        // Test addition
        System.out.println("(" + z1 + ") + (" + z2 + ") = " + z1.add(z2));

        // Test subtraction: (4 - 3) + (2 - (-1))i = 1 + 3i
        System.out.println("(" + z1 + ") - (" + z2 + ") = " + z1.substract(z2));

        // Test multiplication: (4*3 - 2*(-1)) + (4*(-1) + 2*3)i = 14 + 2i
        System.out.println("(" + z1 + ") * (" + z2 + ") = " + z1.multiply(z2));

        // Test division: (4 + 2i) / (3 - 1i)
        // c^2 + d^2 = 9 + 1 = 10
        // Real: (4*3 + 2*(-1))/10 = 10/10 = 1.0
        // Imag: (2*3 - 4*(-1))/10 = 10/10 = 1.0
        System.out.println("(" + z1 + ") / (" + z2 + ") = " + z1.divide(z2));
    }
}
```

<details markdown="block">
<summary>Expected Output for ComplexNum Arithmetic</summary>

```text
(4.0 + 2.0 i) + (3.0 + -1.0 i) = 7.0 + 1.0 i
(4.0 + 2.0 i) - (3.0 + -1.0 i) = 1.0 + 3.0 i
(4.0 + 2.0 i) * (3.0 + -1.0 i) = 14.0 + 2.0 i
(4.0 + 2.0 i) / (3.0 + -1.0 i) = 1.0 + 1.0 i
```
</details>

---

### Task 2: Complete `StepTracker.java` Methods (2019 AP CSA FRQ 1)
Hard
{: .label .label-red }

Now implement the three core behavioral methods of `StepTracker.java`.

#### 1. Method `addDailySteps(int steps)`
* **Signature:** `public void addDailySteps(int steps)`
* **Role:** A **mutator method** that logs a day's physical activity.
* **Algorithm:**
  1. Add `steps` to `this.totalStep`.
  2. Increment `this.totalDays` by 1.
  3. If `steps >= this.minimumSteps`, increment `this.activeDays` by 1.

#### 2. Method `activeDays()`
* **Signature:** `public int activeDays()`
* **Role:** An **accessor method** that returns the number of active days logged so far.
* **Return Value:** `return this.activeDays;`

#### 3. Method `averageSteps()`
* **Signature:** `public double averageSteps()`
* **Role:** A **computational method** that calculates the average steps per day.
* **Edge Case Handling:**
  * If no days have been logged yet (`totalDays == 0`), return `0.0`. (Do NOT divide by zero!).
* **Casting Mandate:**
  * Both `totalStep` and `totalDays` are integers. You **must cast to `double`** to avoid integer truncation:
    ```java
    return (double) this.totalStep / this.totalDays;
    ```

#### Starter Code Skeleton
```java
package ch3_1;

public class StepTracker {

    private int minimumSteps;
    private int activeDays;
    private int totalDays;
    private int totalStep;

    // Constructors from Lab 2
    public StepTracker(int minimumSteps) {
        if (minimumSteps < 0) {
            throw new IllegalArgumentException("Minimum steps cannot be negative: " + minimumSteps);
        }
        this.minimumSteps = minimumSteps;
        this.totalDays = 0;
        this.totalStep = 0;
        this.activeDays = 0;
    }

    public StepTracker() {
        this(10000);
    }

    // TODO 1: Implement addDailySteps(int steps)
    // - Add steps to totalStep
    // - Increment totalDays
    // - If steps >= minimumSteps, increment activeDays


    // TODO 2: Implement activeDays()
    // - Return the count of active days


    // TODO 3: Implement averageSteps()
    // - Guard: if totalDays == 0, return 0.0
    // - Return (double) totalStep / totalDays


    // Getters from Lab 1
    public int getMinimumSteps() { return this.minimumSteps; }
    public int getTotalStep() { return this.totalStep; }
    public int getTotalDays() { return this.totalDays; }
}
```

---

### Task 3: Official College Board AP Exam Simulation

Verify your `StepTracker` implementation against the canonical test trace directly from the **2019 AP Computer Science A Exam Scoring Guidelines**:

```java
package ch3_1;

public class StepTrackerVerification {
    public static void main(String[] args) {
        StepTracker tr = new StepTracker(10000);

        // 1. Initial state
        System.out.println("Active days (expect 0): " + tr.activeDays());
        System.out.println("Average steps (expect 0.0): " + tr.averageSteps());

        // 2. Day 1: 9000 steps (not active)
        tr.addDailySteps(9000);

        // 3. Day 2: 5000 steps (not active)
        tr.addDailySteps(5000);
        System.out.println("Active days (expect 0): " + tr.activeDays());
        System.out.println("Average steps (expect 7000.0): " + tr.averageSteps());

        // 4. Day 3: 13000 steps (active!)
        tr.addDailySteps(13000);
        System.out.println("Active days (expect 1): " + tr.activeDays());
        System.out.println("Average steps (expect ~9000.0): " + tr.averageSteps());

        // 5. Day 4: 23000 steps (active!)
        tr.addDailySteps(23000);

        // 6. Day 5: 1111 steps (not active)
        tr.addDailySteps(1111);
        System.out.println("Active days (expect 2): " + tr.activeDays());
        System.out.println("Average steps (expect 10222.2): " + tr.averageSteps());
    }
}
```

<details markdown="block">
<summary>Expected Output</summary>

```text
Active days (expect 0): 0
Average steps (expect 0.0): 0.0
Active days (expect 0): 0
Average steps (expect 7000.0): 7000.0
Active days (expect 1): 1
Average steps (expect ~9000.0): 9000.0
Active days (expect 2): 2
Average steps (expect 10222.2): 10222.2
```
</details>

---

## Unit 3 Class Blueprint Milestones

Congratulations! You have constructed two complete, professional-grade classes from scratch across Labs 1, 2, and 3:

```
+-------------------------------------------------------------------+
|                        CLASS CREATION ROADMAP                     |
+-------------------------------------------------------------------+
|  Lab 1: State & Encapsulation                                     |
|         - private instance variables                              |
|         - Getters & Setters                                       |
|         - The 'this' keyword                                      |
+-------------------------------------------------------------------+
|  Lab 2: Object Initialization                                     |
|         - Constructors matching class name                        |
|         - Constructor Overloading                                 |
|         - Precondition Validation (IllegalArgumentException)      |
|         - Constructor Chaining: this(...)                         |
+-------------------------------------------------------------------+
|  Lab 3: Behaviors & Calculations                                  |
|         - Pure methods returning new objects (add, divide)        |
|         - Mutator methods updating state (addDailySteps)          |
|         - Type casting & zero-division edge cases                 |
+-------------------------------------------------------------------+
```

---

## Looking Ahead to Lab 4
You have mastered creating text-based classes. Now, in **Unit 3 Lab 4 (Capstone)**, you will take everything you have learned and apply it to an interactive 3D application: **The Virtual Museum Gallery** using Processing!
