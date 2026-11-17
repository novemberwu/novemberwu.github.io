---
title: Unit 3 Lab 2 - Constructors & Object Initialization
layout: page
nav_order: 14
parent: 2026csa
math: mathjax
---

# Unit 3 Lab 2: Constructors & Object Initialization
{: .no_toc }

## Goals
{: .no_toc }
* Understand the role of **constructors** in bringing Java objects to life on the heap.
* Master the syntax rules of constructors: exact class name match, **no return type** (not even `void`), and access modifiers.
* Write **no-argument (default) constructors** and **parameterized constructors**.
* Master **constructor overloading**: defining multiple constructors with distinct parameter lists.
* Implement **defensive validation** in constructors (preconditions, bounds checking, and throwing `IllegalArgumentException`).
* Use **constructor chaining (`this(...)`)** to eliminate redundant initialization logic across constructors.
* Understand the Java compiler's **automatic default constructor rule**.
* Complete constructor implementations for `ComplexNum.java` and `StepTracker.java`.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Concepts & Constructor Fundamentals

In Lab 1, you defined the fields that store an object's state. But how does that state get set when an object is first created with the `new` operator? That is the job of a **constructor**.

```
Client Code                     Java Memory (Heap)
-------------------------       ---------------------------------------
Fraction f = new Fraction(3, 4);
                 |
                 +---------> [ Allocate memory for new Fraction object ]
                             [ Run Fraction(3, 4) constructor body     ]
                             [ Set num = 3, denom = 4                  ]
                             [ Return reference address to variable f  ]
```

### 1. Syntax Rules for Constructors
A constructor looks similar to a method, but has three strict differences:

1. **Exact Name Match:** The constructor name must match the class name **identically**, including uppercase letters (`public Fraction(...)`).
2. **NO Return Type:** A constructor has **no return type** — not even `void`!
   * *Fatal AP Mistake:* Writing `public void Fraction(int n, int d)` is legally treated by Java as a regular method named `Fraction`, **not** a constructor. You will get mysterious compilation errors when trying `new Fraction(3, 4)`.
3. **Triggered Exclusively by `new`:** You cannot call a constructor like an ordinary method on an existing object. It runs only when `new ClassName(...)` is executed.

### 2. The Default Constructor Rule
* If you write **zero constructors** in a class, the Java compiler automatically supplies an invisible, no-argument default constructor that initializes primitives to `0`/`false` and reference types to `null`.
* **The Trap:** The instant you write **any constructor** (such as a 2-argument constructor), the compiler **stops** providing the automatic default constructor. If client code needs `new ClassName()`, you must explicitly write a no-args constructor yourself!

### 3. Constructor Overloading
A class can have multiple constructors as long as their parameter lists have different numbers of parameters or different data types. This allows clients to initialize objects in various convenient ways:

```java
Fraction f1 = new Fraction();        // 0 / 1
Fraction f2 = new Fraction(5);       // 5 / 1
Fraction f3 = new Fraction(3, 4);    // 3 / 4
```

### 4. Constructor Chaining with `this(...)`
When one constructor's logic is a special case of another, you can call one constructor from another using `this(...)`:

```java
public Fraction() {
    this(0, 1); // Calls the 2-argument constructor Fraction(0, 1)
}
```

{: .note }
> **Syntax Mandate:** If you use `this(...)` to call another constructor, it **must be the very first statement** inside the constructor body.

---

## Part 2: Worked Example — `Fraction.java`

Let's examine how professional constructors are designed by studying `Fraction.java` from `~/src/csa/ch3_1`.

### The 4 Overloaded Constructors of `Fraction`

```java
package ch3_1;

public class Fraction {
    private int num;
    private int denom;

    // ---------------------------------------------------------
    // Constructor 1: No-args constructor (Default Rational Zero)
    // ---------------------------------------------------------
    public Fraction() {
        num = 0;
        denom = 1;
    }

    // ---------------------------------------------------------
    // Constructor 2: Whole Number Conversion (e.g. 5 -> 5/1)
    // ---------------------------------------------------------
    public Fraction(int n) {
        num = n;
        denom = 1;
    }

    // ---------------------------------------------------------
    // Constructor 3: Full Parameterized Constructor with Validation
    // ---------------------------------------------------------
    public Fraction(int n, int d) {
        if (d != 0) {
            num = n;
            denom = d;
            reduce(); // Private helper method that simplifies fraction
        } else {
            // Defensive programming: prevent corrupt object state!
            throw new IllegalArgumentException(
                "Fraction construction error: denominator is 0");
        }
    }

    // ---------------------------------------------------------
    // Constructor 4: Copy Constructor
    // Creates a new, distinct Fraction object with identical values
    // ---------------------------------------------------------
    public Fraction(Fraction other) {
        num = other.num;
        denom = other.denom;
    }
}
```

### Key Takeaways from the `Fraction` Example:

1. **Defensive Validation (Preconditions):**
   A fraction with denominator 0 is mathematically undefined. Rather than letting the object sit corrupted in memory, constructor 3 inspects `d != 0`. If `d == 0`, it rejects creation immediately:
   ```java
   throw new IllegalArgumentException("Fraction construction error: denominator is 0");
   ```
2. **Canonical State (`reduce()`):**
   The constructor simplifies $$\frac{12}{-20}$$ into canonical reduced form $$-\frac{3}{5}$$ upon creation. An object's constructor is responsible for establishing its valid initial invariant.
3. **Copy Constructor:**
   `public Fraction(Fraction other)` takes an existing `Fraction` and copies its fields. This produces an independent clone so modifying one does not affect the other.

#### Verifying with `TestFractions.java`
```java
package ch3_1;

public class TestFractions {
    public static void testConstructors() {
        Fraction f1 = new Fraction();         // Invokes Constructor 1
        Fraction f2 = new Fraction(7);        // Invokes Constructor 2
        Fraction f3 = new Fraction(12, -20);  // Invokes Constructor 3
        Fraction f4 = new Fraction(f3);       // Invokes Constructor 4

        System.out.println("f1 = " + f1); // f1 = 0/1
        System.out.println("f2 = " + f2); // f2 = 7/1
        System.out.println("f3 = " + f3); // f3 = -3/5 (auto-reduced!)
        System.out.println("f4 = " + f4); // f4 = -3/5
    }

    public static void main(String[] args) {
        testConstructors();
    }
}
```

---

## Part 3: Student Tasks

Now continue developing your two classes by implementing their constructors!

---

### Task 1: Complete `ComplexNum.java` Constructors
Medium
{: .label .label-yellow }

Open `ComplexNum.java` from Lab 1. We want to support three ways of creating a complex number:
1. `new ComplexNum()`: represents $$0 + 0i$$.
2. `new ComplexNum(3.5)`: represents a purely real number $$3.5 + 0i$$.
3. `new ComplexNum(2.0, -4.5)`: represents $$2.0 - 4.5i$$.
4. *(Bonus)* A copy constructor `new ComplexNum(existingNum)`.

#### Requirements
1. **Full 2-Argument Constructor:**
   * **Signature:** `public ComplexNum(double real, double imaginary)`
   * Initializes `this.realPart` and `this.imaginaryPart`.
2. **No-Args Constructor with Chaining:**
   * **Signature:** `public ComplexNum()`
   * Use constructor chaining `this(...)` to call the 2-argument constructor with `0.0` and `0.0`.
3. **Single-Argument Constructor (The TODO from repo):**
   * **Signature:** `public ComplexNum(double real)`
   * Initializes `realPart` to `real` and `imaginaryPart` to `0.0`. (Tip: use constructor chaining `this(real, 0.0);`).
4. **Copy Constructor (AP CSA Extension):**
   * **Signature:** `public ComplexNum(ComplexNum other)`
   * Copies `other.realPart` and `other.imaginaryPart` into this object.

#### Starter Code Skeleton
```java
package ch3_1;

public class ComplexNum {

    private double realPart;
    private double imaginaryPart;

    // TODO 1: Implement full constructor: ComplexNum(double real, double imaginary)


    // TODO 2: Implement no-args constructor using chaining: ComplexNum()


    // TODO 3: Implement single-argument constructor: ComplexNum(double real)


    // TODO 4: Implement copy constructor: ComplexNum(ComplexNum other)


    // Getters, Setters, and toString from Lab 1...
    public double getRealPart() { return realPart; }
    public double getImaginaryPart() { return imaginaryPart; }
    public void setRealPart(double realPart) { this.realPart = realPart; }
    public void setImaginaryPart(double imaginaryPart) { this.imaginaryPart = imaginaryPart; }

    public String toString() {
        return realPart + " + " + imaginaryPart + " i";
    }
}
```

#### Client Verification (`ComplexNumClient.java`)
```java
package ch3_1;

public class ComplexNumClient {
    public static void main(String[] args) {
        ComplexNum c1 = new ComplexNum();
        ComplexNum c2 = new ComplexNum(5.5);
        ComplexNum c3 = new ComplexNum(2.0, -3.0);
        ComplexNum c4 = new ComplexNum(c3);

        System.out.println("c1 (default): " + c1);
        System.out.println("c2 (real only): " + c2);
        System.out.println("c3 (full): " + c3);
        System.out.println("c4 (copy of c3): " + c4);

        // Verify independent copy
        c3.setRealPart(99.0);
        System.out.println("After mutating c3, c4 is unaffected: " + c4.getRealPart());
    }
}
```

<details markdown="block">
<summary>Expected Output for Task 1</summary>

```text
c1 (default): 0.0 + 0.0 i
c2 (real only): 5.5 + 0.0 i
c3 (full): 2.0 + -3.0 i
c4 (copy of c3): 2.0 + -3.0 i
After mutating c3, c4 is unaffected: 2.0
```
</details>

---

### Task 2: Implement `StepTracker.java` Constructors
Medium
{: .label .label-yellow }

Now implement the constructors for `StepTracker.java`.

#### Requirements
1. **Primary Parameterized Constructor (Official AP CSA Specification):**
   * **Signature:** `public StepTracker(int minimumSteps)`
   * **Behavior:**
     * Initializes `this.minimumSteps` to the passed parameter value.
     * Explicitly initializes all accumulator fields to zero:
       * `this.totalStep = 0;`
       * `this.totalDays = 0;`
       * `this.activeDays = 0;`
     * **Validation Check:** Steps cannot be negative. If `minimumSteps < 0`, throw an `IllegalArgumentException("Minimum steps cannot be negative: " + minimumSteps)`.
2. **Overloaded Default Constructor:**
   * **Signature:** `public StepTracker()`
   * **Behavior:** Chains to the main constructor using `this(10000);` (the standard CDC daily step recommendation).

#### Starter Code Skeleton
```java
package ch3_1;

public class StepTracker {

    private int minimumSteps;
    private int activeDays;
    private int totalDays;
    private int totalStep;

    // TODO 1: Implement primary constructor: StepTracker(int minimumSteps)
    // - If minimumSteps < 0, throw IllegalArgumentException
    // - Initialize this.minimumSteps
    // - Explicitly initialize totalDays, totalStep, activeDays to 0


    // TODO 2: Implement overloaded constructor: StepTracker()
    // - Use constructor chaining this(...) to default to 10000 steps


    // Getters from Lab 1
    public int getMinimumSteps() { return this.minimumSteps; }
    public int getTotalStep() { return this.totalStep; }
    public int getTotalDays() { return this.totalDays; }
    public int getActiveDays() { return this.activeDays; }
}
```

#### Client Verification (`StepTrackerClient.java`)
```java
package ch3_1;

public class StepTrackerClient {
    public static void main(String[] args) {
        StepTracker athleteTracker = new StepTracker(15000);
        StepTracker defaultTracker = new StepTracker();

        System.out.println("Athlete Goal: " + athleteTracker.getMinimumSteps());
        System.out.println("Default Goal: " + defaultTracker.getMinimumSteps());

        // Test exception handling
        try {
            StepTracker invalidTracker = new StepTracker(-500);
            System.out.println("ERROR: Should not reach here!");
        } catch (IllegalArgumentException e) {
            System.out.println("Caught expected exception: " + e.getMessage());
        }
    }
}
```

<details markdown="block">
<summary>Expected Output for Task 2</summary>

```text
Athlete Goal: 15000
Default Goal: 10000
Caught expected exception: Minimum steps cannot be negative: -500
```
</details>

---

## Memory Tracing: Stack vs Heap

Consider the following execution:

```java
StepTracker trackerA = new StepTracker(10000);
StepTracker trackerB = new StepTracker(5000);
```

### Memory Diagram:
```
STACK FRAME (main)                     HEAP MEMORY
-------------------                    -----------------------------------
trackerA (ref: 0x88F0) --------------> [ Object StepTracker @ 0x88F0    ]
                                       | minimumSteps = 10000           |
                                       | totalStep = 0                  |
                                       | totalDays = 0                  |
                                       | activeDays = 0                 |
                                       +--------------------------------+

trackerB (ref: 0x99A4) --------------> [ Object StepTracker @ 0x99A4    ]
                                       | minimumSteps = 5000            |
                                       | totalStep = 0                  |
                                       | totalDays = 0                  |
                                       | activeDays = 0                 |
                                       +--------------------------------+
```

* `trackerA` and `trackerB` are **reference variables** located in stack memory.
* Each points to its own distinct object in heap memory. Modifying `trackerA`'s steps will have **zero effect** on `trackerB`.

---

## Self-Check AP CSA Questions

1. What happens if you define a method `public void ComplexNum(double r)` inside `ComplexNum`?
   * *Answer:* Because it specifies a return type (`void`), the Java compiler treats it as a regular method, NOT a constructor. When client code runs `new ComplexNum(3.0)`, the code will fail to compile because no matching constructor exists.
2. In constructor chaining, where must `this(...)` appear?
   * *Answer:* It must be the very first line of code in the constructor body.
3. If a class defines `public StepTracker(int m)` but does not define `public StepTracker()`, can client code call `new StepTracker()`?
   * *Answer:* **No.** The compiler only provides an automatic no-args constructor if the class defines *no constructors at all*.

---

## Looking Ahead to Lab 3
Now that our objects can be safely constructed with customized starting values, they need **Behaviors**! In **Unit 3 Lab 3**, you will write **Instance Methods** to perform complex number arithmetic (`add`, `multiply`, `substract`, `divide`) and step tracking calculations (`addDailySteps`, `activeDays`, `averageSteps`)!
