---
title: Unit 3 Lab 1 - Class Anatomy & Encapsulation
layout: page
nav_order: 13
parent: 2026csa
math: mathjax
---

# Unit 3 Lab 1: Class Anatomy & Encapsulation
{: .no_toc }

## Goals
{: .no_toc }
* Understand the blueprint role of a Java class and the anatomy of a class declaration.
* Learn how to declare **instance variables** (fields) to represent the attributes/state of an object.
* Master the core Object-Oriented principle of **Encapsulation**: why instance variables should almost always be `private`.
* Distinguish between **mutable** state (values that change over time) and **immutable** state (using the `final` keyword).
* Implement standard **accessor methods (getters)** to provide safe, read-only access to private state.
* Implement standard **mutator methods (setters)** to provide controlled, validated write access to state.
* Understand variable shadowing and use the `this` keyword to disambiguate field references (`this.variableName = variableName;`).
* Begin building two complete AP CSA classes from scratch: `ComplexNum` and `StepTracker`.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Concepts & The Anatomy of a Class

In **Unit 1 (Elementary Programming)** and **Unit 2 (If/Else & Loops)**, you learned how to write control logic and *use* existing classes like `String` and `Math`. In **Unit 3 (Creating Classes)**, you become the **creator**. You will design your own blueprints to model real-world concepts, mathematical structures, and data systems.

### 1. What is a Class?
A **class** in Java is a programmer-defined blueprint (or template) from which individual **objects** (instances) are created. 

Every class consists of three primary components:
1. **State (Instance Variables / Attributes):** What data does an object know about itself?
2. **Initialization (Constructors):** How is an object created and its initial state configured? (Covered in Lab 2)
3. **Behavior (Instance Methods):** What operations or actions can an object perform on its state? (Covered in Lab 3)

```
+--------------------------------------------------------------+
|                        Class Header                          |
|         public class ClassName {                             |
|                                                              |
|   1. Instance Variables (State)  --> What the object HAS     |
|      - private int id;                                       |
|      - private String name;                                  |
|                                                              |
|   2. Constructors (Setup)        --> How the object is BORN  |
|      - public ClassName(...) { ... }                         |
|                                                              |
|   3. Instance Methods (Behavior) --> What the object DOES    |
|      - Getters & Setters                                     |
|      - Calculation & Business Logic                          |
|   }                                                          |
+--------------------------------------------------------------+
```

### 2. The Class Header
The class declaration begins with the header:

```java
public class ProductReview {
    // class body
}
```

* `public`: The access modifier. A public class is accessible to other classes in your program or package.
* `class`: The Java reserved keyword indicating you are defining a new reference type.
* `ProductReview`: The class identifier. By Java convention, class names use **UpperCamelCase** (noun or noun phrase). The file name must match the class name exactly: `ProductReview.java`.

### 3. Instance Variables & Encapsulation
An **instance variable** is a variable declared inside the class body but *outside* any constructor or method. Every object created from the class receives its own independent copy of these variables in memory on the heap.

```java
public class ProductReview {
    private final int rating;
    private String text;
    private final String authorId;
    private final String productId;
}
```

#### Why `private`? (The Encapsulation Guarantee)
In AP CSA, **all instance variables must be declared `private`**.
* If a variable were `public`, any outside code could tamper with the internal state:
  ```java
  // DANGEROUS: If rating was public!
  review.rating = -999; 
  review.authorId = null;
  ```
* Declaring variables `private` enforces **information hiding**. The outside world cannot read or modify the variable directly. Any interaction must go through explicit `public` methods written by you.

#### The `final` Keyword (Immutability)
Notice `private final int rating;` and `private final String authorId;`.
* A variable marked `final` can only be assigned a value **once** (usually inside a constructor). After assignment, its value is frozen.
* In real-world software, identifying which fields should be immutable prevents bugs:
  * **Immutable:** An author's ID or product ID should never change after a review is created.
  * **Mutable:** The review `text` might be edited or updated by the user later, so it should **not** be `final`.

### 4. Accessor Methods (Getters)
To allow outside code to read private data without allowing unauthorized modification, we write an **accessor method** (commonly called a **getter**).

```java
public int getRating() {
    return this.rating;
}

public String getText() {
    return this.text;
}
```

* **Naming Convention:** `get` followed by the capitalized field name (e.g., `getRating()`, `getText()`).
* **Return Type:** Matches the type of the private variable.
* **Parameters:** None (`()`).
* **The `this` Keyword:** Refers to the current calling object (`this.rating`). While often optional when there is no naming collision, using `this.` explicitly clarifies that you are referring to an instance variable.

### 5. Mutator Methods (Setters)
To allow outside code to change a mutable private variable, we write a **mutator method** (commonly called a **setter**).

```java
public void setText(String t) {
    if (t != null && !t.trim().isEmpty()) {
        this.text = t;
    }
}
```

* **Naming Convention:** `set` followed by the capitalized field name (e.g., `setText(...)`).
* **Return Type:** Almost always `void`.
* **Parameters:** Takes a new value matching the field type.
* **Validation Gatekeeper:** Unlike a bare public variable, a setter can validate arguments before updating the state (e.g., rejecting null or empty strings).

---

## Part 2: Worked Examples from the Codebase

Let's study two complete reference classes from `~/src/csa/ch3_1`: `ProductReview.java` and `Fraction.java`.

### Worked Example 1: `ProductReview.java` & `ReviewClient.java`

Look at how `ProductReview.java` models a commercial e-commerce review:

```java
package ch3_1;

public class ProductReview {
    // 1. Private instance variables
    private final int rating;          // Immutable star rating (1-5)
    private String text;               // Mutable review text
    private final String authorId;     // Immutable reviewer ID
    private final String productId;    // Immutable product ID
    private String[] images;           // Attached photos

    // 2. Accessors (Getters)
    public int getRating() {
        return this.rating;
    }

    public String getText() {
        return this.text;
    }

    public String getAuthorId() {
        return this.authorId;
    }

    public String getProductId() {
        return this.productId;
    }

    // 3. Mutator (Setter) - only provided for mutable fields!
    public void setText(String t) {
        this.text = t;
    }

    // Note: Constructors will be covered in Lab 2
    public ProductReview(int rating, String authorId, String pid) {
        this.rating = rating;
        this.authorId = authorId;
        this.productId = pid;
    }

    public ProductReview() {
        this.rating = 5;
        this.authorId = "anonymous";
        this.productId = "default value";
    }
}
```

#### Client Verification (`ReviewClient.java`)
Now examine how an outside client class uses this encapsulated object:

```java
package ch3_1;

public class ReviewClient {
    public static void main(String[] args) {
        // Instantiate a ProductReview object
        ProductReview review = new ProductReview(5, "rachelwu", "mechanical_keyboard");

        // Read state using getter
        int r = review.getRating();
        System.out.println("Rating: " + r); // Output: Rating: 5

        // Attempting review.rating = 4; causes a COMPILATION ERROR! (rating has private access)

        // Mutate state using setter
        review.setText("Crisp tactile switches and solid aluminum chassis!");
        System.out.println("Review Body: " + review.getText());
    }
}
```

---

### Worked Example 2: `Fraction.java`

Next, inspect `Fraction.java` from `~/src/csa/ch3_1`. A fraction represents a rational number $$\frac{\text{num}}{\text{denom}}$$:

```java
package ch3_1;

public class Fraction {
    // 1. Instance variables
    private int num;
    private int denom;

    // 2. Accessors
    public int getNum() {
        return this.num;
    }

    public int getDenom() {
        return this.denom;
    }

    public double getValue() {
        return (double) this.num / (double) this.denom;
    }

    public String toString() {
        return this.num + "/" + this.denom;
    }

    // Mutator with validation invariant!
    public void setDenom(int d) {
        if (d == 0) {
            throw new IllegalArgumentException("Denominator cannot be zero!");
        }
        this.denom = d;
    }
}
```

#### Why Encapsulation Matters Here
If `denom` were `public`, a user could write:
```java
Fraction f = new Fraction(1, 2);
f.denom = 0; // Catastrophic arithmetic invariant violation!
System.out.println(f.getValue()); // Division by zero crash
```
By making `denom` `private` and controlling modification through `setDenom(int d)`, the class guarantees that `denom` is never allowed to be zero.

---

## Part 3: Student Tasks

Now you will apply these patterns to begin building two foundational classes that we will develop throughout Unit 3 Labs 1, 2, and 3:
1. `ComplexNum.java`: Modeling complex numbers $$a + bi$$.
2. `StepTracker.java`: Modeling a physical activity step counter (AP CSA FRQ domain).

---

### Task 1: Building `ComplexNum.java` (Part 1 - State & Accessors)
Easy
{: .label .label-green }

In mathematics, a complex number $$z$$ has the form:
$$z = a + bi$$
where $$a$$ is the **real part**, $$b$$ is the **imaginary part**, and $$i = \sqrt{-1}$$.

Open or create `ComplexNum.java` in your working directory.

#### Requirements
1. **Class Header:** Declare a public class named `ComplexNum`.
2. **Instance Variables:** Declare two `private` `double` instance variables:
   * `realPart`
   * `imaginaryPart`
3. **Getter Methods:**
   * `public double getRealPart()`: returns the value of `realPart`.
   * `public double getImaginaryPart()`: returns the value of `imaginaryPart`.
4. **Setter Methods:**
   * `public void setRealPart(double realPart)`: updates `this.realPart`. Notice the parameter name is identical to the field name; you **must** use `this.realPart = realPart;` to resolve shadowing!
   * `public void setImaginaryPart(double imaginaryPart)`: updates `this.imaginaryPart`.
5. **String Representation:**
   * Write a `public String toString()` method that returns:
     ```java
     realPart + " + " + imaginaryPart + " i"
     ```

#### Starter Code Skeleton
```java
package ch3_1;

public class ComplexNum {

    // TODO 1: Declare private instance variables for realPart and imaginaryPart


    // TODO 2: Write getRealPart()


    // TODO 3: Write getImaginaryPart()


    // TODO 4: Write setRealPart(double realPart)


    // TODO 5: Write setImaginaryPart(double imaginaryPart)


    // TODO 6: Write toString() method returning: realPart + " + " + imaginaryPart + " i"

}
```

#### Client Verification (`ComplexNumClient.java`)
Create a client runner to test your class:

```java
package ch3_1;

public class ComplexNumClient {
    public static void main(String[] args) {
        ComplexNum c = new ComplexNum();
        
        System.out.println("Default initial real: " + c.getRealPart());
        System.out.println("Default initial imag: " + c.getImaginaryPart());

        c.setRealPart(3.5);
        c.setImaginaryPart(-2.0);

        System.out.println("Updated real: " + c.getRealPart());
        System.out.println("Updated imag: " + c.getImaginaryPart());
        System.out.println("Complex number: " + c);
    }
}
```

<details markdown="block">
<summary>Expected Output for Task 1</summary>

```text
Default initial real: 0.0
Default initial imag: 0.0
Updated real: 3.5
Updated imag: -2.0
Complex number: 3.5 + -2.0 i
```
*(Note: In Java, numbers automatically initialize to `0.0` before constructors run. We will write proper constructors in Lab 2).*
</details>

---

### Task 2: Building `StepTracker.java` (Part 1 - State Design & Encapsulation)
Medium
{: .label .label-yellow }

A fitness tracking company wants to record a user's daily walking activity. An active day is defined as any day where the user walks at least a specified minimum number of steps (e.g., 10,000 steps).

#### Architectural Design Thinking: What State Must Be Stored?
Before typing code, an engineer must decide **which data belongs in instance variables**:
1. `minimumSteps`: The threshold of steps needed for a day to qualify as active (e.g., 10,000).
2. `totalStep`: The cumulative sum of all steps taken across all tracked days.
3. `totalDays`: The total count of days recorded so far.
4. `activeDays`: The count of days where the step count was $$\ge \text{minimumSteps}$$.

{: .important }
> **Crucial AP CSA Architectural Question:** Should we store `double averageSteps;` as an instance variable?
> 
> **NO!** In object-oriented design, you should avoid storing **derived/redundant state**. If you stored `averageSteps` as a field, every time someone adds daily steps, `averageSteps` could become stale or out-of-sync. Instead, `averageSteps()` is calculated on-the-fly in a method by dividing `totalStep` by `totalDays`.

#### Requirements
1. **Class Header:** Declare a public class named `StepTracker`.
2. **Instance Variables:** Declare four `private int` instance variables:
   * `minimumSteps`
   * `totalStep`
   * `totalDays`
   * `activeDays`
3. **Getter Methods:** Write public getter methods for all four instance variables:
   * `public int getMinimumSteps()`
   * `public int getTotalStep()`
   * `public int getTotalDays()`
   * `public int getActiveDays()`
4. **Encapsulation Reflection Question:**
   * Why should you **NOT** write public setters like `setActiveDays(int d)` or `setTotalStep(int s)`?
   * *Answer:* Allowing external classes to directly override step counts breaks the integrity of the fitness tracker! The only way those counts should ever change is when the user actually records a day's workout via `addDailySteps(int steps)` (which we will implement in Lab 3).

#### Starter Code Skeleton
```java
package ch3_1;

public class StepTracker {

    // TODO 1: Declare private int instance variables:
    // minimumSteps, totalStep, totalDays, activeDays


    // TODO 2: Write getMinimumSteps()


    // TODO 3: Write getTotalStep()


    // TODO 4: Write getTotalDays()


    // TODO 5: Write getActiveDays()

}
```

#### Client Verification (`StepTrackerClient.java`)
```java
package ch3_1;

public class StepTrackerClient {
    public static void main(String[] args) {
        StepTracker tracker = new StepTracker();

        System.out.println("Initial minimum steps: " + tracker.getMinimumSteps());
        System.out.println("Initial total steps: " + tracker.getTotalStep());
        System.out.println("Initial total days: " + tracker.getTotalDays());
        System.out.println("Initial active days: " + tracker.getActiveDays());
    }
}
```

<details markdown="block">
<summary>Expected Output for Task 2</summary>

```text
Initial minimum steps: 0
Initial total steps: 0
Initial total days: 0
Initial active days: 0
```
</details>

---

## Self-Check Questions & AP Exam Tips

1. **AP Tip:** When you write an AP CSA Free Response Question (FRQ) that asks you to write an entire class, you receive **1 full point** just for writing the correct class header and declaring all required instance variables as `private` with appropriate types.
2. What is the difference between a local variable declared inside a method and an instance variable?
   * *Local variables* only exist while that method is running, and disappear when the method finishes.
   * *Instance variables* persist for the entire lifetime of the object and are accessible to all instance methods within that class.
3. If an instance variable is declared `private int count;`, what happens if code in another class writes `myObj.count = 5;`?
   * The Java compiler produces an error: `count has private access in ...`.
4. What is the purpose of the `this` keyword?
   * `this` refers to the current calling instance. It resolves ambiguity when a method parameter shares the same name as an instance variable.

---

## Looking Ahead to Lab 2
In Lab 1, your objects defaulted to zeroes and nulls. In **Unit 3 Lab 2**, you will write **Constructors** to initialize `ComplexNum` and `StepTracker` with custom, validated starting values!
