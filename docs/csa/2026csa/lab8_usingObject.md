---
title: Lab 8 Using Classes
layout: page
nav_order: 8
parent: 2026csa
math: mathjax
---

# Lab 8: Using Objects & Classes in Java
{: .no_toc }

## Goals
{: .no_toc}
* Understand the core distinction between a **Class** (the structural blueprint) and an **Object** (the runtime instance with concrete state).
* Master how to **use existing objects**: constructing instances using the `new` keyword and invoking instance methods using the **dot operator (`.`)**.
* Explore the **Color Abstract Data Type (ADT)** and investigate Josef Albers' classic color perception experiment (**Albers Squares**).
* Trace the **3-step object creation lifecycle** in memory: Heap allocation, constructor field initialization, and reference assignment.
* Learn how to **create custom data types**: declaring `private` instance variables, defining **overloaded constructors**, and implementing instance methods.
* Master the principles of **Encapsulation** and **Information Hiding**, understanding compiler access boundaries (`public` vs. `private`).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Class vs. Object (The Conceptual Foundation)

Object-Oriented Programming (OOP) is centered around two fundamental building blocks: **Classes** and **Objects** (Slide 3).

### 1. Blueprint vs. Concrete Instance (Slide 4)
Think of a **Class** as an architectural blueprint or factory mold, and an **Object** as a tangible item manufactured from that mold:

| Concept | The Blueprint (**Class**) | The Concrete Entity (**Object**) |
| :--- | :--- | :--- |
| **Real-World Analogy** | Automotive design blueprint (`Car` Class) | A yellow sports car on the street (`Mustang` Object) |
| **Attributes (Fields)** | Defines *what kind* of data is stored:<br>• `String model`<br>• `Color color`<br>• `int numPassengers`<br>• `double amountOfGas` | Holds *specific values* for each attribute:<br>• `model = "Mustang"`<br>• `color = Color.YELLOW`<br>• `numPassengers = 0`<br>• `amountOfGas = 16.5` |
| **Behaviors (Methods)** | Defines *what actions* are possible:<br>• Add / remove a passenger<br>• Fill the gas tank<br>• Report when fuel is low | Performs actions that update or query its state:<br>• `addPassenger()` updates `numPassengers`<br>• `fillTank()` restores `amountOfGas`<br>• `checkGas()` returns fuel status |

### 2. Code Definition vs. Runtime Execution (Slide 5, 6)
* **Class (Source Code):**
  * A piece of program source code written by a programmer.
  * Specifies the structural template: the number and types of its objects' attributes (which is identical for all objects of that class).
  * Specifies the available behaviors (methods) of its objects.
* **Object (Runtime Entity):**
  * An entity that exists in memory while the program is running.
  * Created at runtime when the program executes (by `main` or another method).
  * Holds specific values for its attributes, and those values can change over time as the program executes.

---

## Part 2: Using Existing Objects & The Color ADT

To **use** an existing data type or class in Java, you only need to know three things (Slide 8):
1. **Its Name:** Capitalized in Java (e.g., `String`, `Color`, `Building`).
2. **How to construct new objects:** Use the keyword **`new`** followed by the class name to invoke a constructor (e.g., `new Building()`).
3. **How to apply operations (invoke methods):** Use the **object variable name**, the **dot operator (`.`)**, and the **method name** with arguments:
   ```java
   String s = new String("Hello, World");
   System.out.println(s.substring(0, 5)); // Calls substring on object s
   ```

### What is an Abstract Data Type (ADT)? (Slide 7, 9)
An **Abstract Data Type (ADT)** is a data type whose internal implementation details are hidden from the user, exposing only an **Application Programming Interface (API)** of operations.

A prime example is the **`Color` ADT** (Slide 9):
* Color is represented digitally using 24 bits: 8 bits each for **Red (R)**, **Green (G)**, and **Blue (B)** intensity, ranging from `0` (no intensity) to `255` (full intensity).
* The Java `Color` class (`java.awt.Color`) encapsulates these values and exposes a clean public API:

```
public class Color
-------------------------------------------------------------------------------------
       Color(int r, int g, int b)   // Constructor: creates a color with RGB values [0..255]
   int getRed()                     // Returns red intensity (0..255)
   int getGreen()                   // Returns green intensity (0..255)
   int getBlue()                    // Returns blue intensity (0..255)
 Color brighter()                   // Returns a brighter version of this color
 Color darker()                     // Returns a darker version of this color
String toString()                   // Returns string representation of this color
boolean equals(Color c)             // Returns true if this color is identical to c
```

---

## Example A: Albers Squares (Optical Color Perception)

Josef Albers (1888–1976) was a revolutionary 20th-century artist and Bauhaus educator who explored how human perception of color is relative rather than absolute (Slide 10). 

In his famous *Interaction of Color* studies, Albers demonstrated that placing two identical color squares inside two differently colored backgrounds tricks human eyes into perceiving the center squares as two completely different colors (Slide 11)!

Let's study the program `AlbersSquares.java` (Slide 11, 12). It takes 6 command-line integers representing two RGB colors ($$c_1$$ and $$c_2$$), instantiates two `Color` objects, and draws nested squares using `StdDraw`:

```java
import java.awt.Color;

public class AlbersSquares {
    public static void main(String[] args) {
        // Parse RGB values for first color (c1)
        int r1 = Integer.parseInt(args[0]);
        int g1 = Integer.parseInt(args[1]);
        int b1 = Integer.parseInt(args[2]);
        Color c1 = new Color(r1, g1, b1); // Create first Color object

        // Parse RGB values for second color (c2)
        int r2 = Integer.parseInt(args[3]);
        int g2 = Integer.parseInt(args[4]);
        int b2 = Integer.parseInt(args[5]);
        Color c2 = new Color(r2, g2, b2); // Create second Color object

        // Draw left square: c1 outer background, c2 inner square
        StdDraw.setPenColor(c1);
        StdDraw.filledSquare(0.25, 0.5, 0.2); // Outer square with half-width 0.2
        StdDraw.setPenColor(c2);
        StdDraw.filledSquare(0.25, 0.5, 0.1); // Inner square with half-width 0.1

        // Draw right square: c2 outer background, c1 inner square
        StdDraw.setPenColor(c2);
        StdDraw.filledSquare(0.75, 0.5, 0.2); // Outer square with half-width 0.2
        StdDraw.setPenColor(c1);
        StdDraw.filledSquare(0.75, 0.5, 0.1); // Inner square with half-width 0.1
    }
}
```

### Running AlbersSquares:
```bash
# Example 1: Dark Blue vs. Medium Gray
java AlbersSquares 0 64 128 105 105 105

# Example 2: Vibrant Orange vs. Olive Gold
java AlbersSquares 251 112 34 177 153 71

# Example 3: Forest Green vs. Sea Green
java AlbersSquares 28 183 122 15 117 123
```

Notice how we created two distinct instances of `Color` (`c1` and `c2`) and passed them as arguments to `StdDraw.setPenColor()`. This is procedural abstraction and object usage in action!

---

## Task A: Color ADT & Luminance Explorer

Easy
{: .label .label-green }

Now, test your understanding of using existing objects and the `Color` ADT by writing a client program that creates color objects, inspects their RGB components, and tests color brightness transformations.

In computer vision, the **monochrome luminance** (perceived brightness) of a color can be calculated from its RGB components using the formula:

$$\text{Luminance} = 0.299 \times R + 0.587 \times G + 0.114 \times B$$

### Instructions:
1. Create a file named `ColorExplorer.java`.
2. Import `java.awt.Color`.
3. Inside `main`:
   * Construct a `Color` object named `navy` with RGB values $$(0, 64, 128)$$.
   * Construct a `Color` object named `brightNavy` by calling `.brighter()` on `navy`.
   * Print the RGB values of `navy` using `.getRed()`, `.getGreen()`, and `.getBlue()`.
   * Calculate and print the luminance of `navy` using the formula above as a `double`.
   * Print whether `navy` equals `brightNavy` using `.equals()`.

### Code Skeleton:
```java
import java.awt.Color;

public class ColorExplorer {
    public static void main(String[] args) {
        // TODO 1: Construct the 'navy' Color object with RGB values (0, 64, 128)
        

        // TODO 2: Create a brighter version 'brightNavy' by calling .brighter() on navy
        

        // TODO 3: Print the RGB components of navy and brightNavy using .getRed(), .getGreen(), and .getBlue()
        

        // TODO 4: Calculate luminance of navy using formula: 0.299 * R + 0.587 * G + 0.114 * B
        

        // TODO 5: Check if navy equals brightNavy using .equals() and print the result
        
    }
}
```

### Expected Output:
```
Navy RGB: (0, 64, 128)
Brighter Navy RGB: (0, 91, 182)
Navy Luminance: 52.16
Are navy and brightNavy equal? false
```

<details markdown="1">
<summary><b>Click to check your understanding of Object Usage:</b></summary>

1. **Why does `navy.brighter()` return a NEW `Color` object instead of modifying `navy`?**
   * In Java, `Color` objects are **immutable**. Once created, their RGB values cannot be changed. Methods like `.brighter()` return a freshly instantiated `Color` object with adjusted values.
2. **What does the `==` operator do if you write `navy == brightNavy`?**
   * `==` checks if both reference variables point to the exact same memory address. Since they are two distinct objects on the heap, `==` evaluates to `false`. `.equals()` checks whether the RGB values are identical.

</details>

---

## Part 3: The 3-Step Lifecycle of Object Creation

When you write a statement that instantiates an object:
```java
Car c = new Car("Yellow", 2025, "Tesla", "Model Y");
```
Java performs three distinct steps in memory behind the scenes (Slides 15, 16, 17):

```
       Step 1: Allocate Memory (Heap)
       -------------------------------
       Car instance created with defaults:
       color = null, year = 0, make = null, model = null
                     │
                     ▼
       Step 2: Constructor Initializes Data
       ------------------------------------
       Constructor executes:
       color = "Yellow", year = 2025, make = "Tesla", model = "Model Y"
                     │
                     ▼
       Step 3: Assign Address to Reference Variable (Stack)
       ----------------------------------------------------
       Variable 'c' stores the memory address pointing to the Car object
```

### Step 1: Allocate Memory on the Heap (Slide 15)
The keyword **`new`** tells the Java Virtual Machine (JVM) to allocate a block of memory on the **Heap** large enough to store all instance variables of the class. 

At this exact moment (before the constructor statements execute), Java initializes all instance fields to their **default zero values**:
* Numeric primitives (`int`, `double`): `0` or `0.0`
* Booleans: `false`
* Object references (`String`, `Color`): `null`

### Step 2: Constructor Initializes Data (Slide 16)
Java jumps into the class's **constructor**. The arguments passed in (`"Yellow"`, `2025`, `"Tesla"`, `"Model Y"`) are received as parameter variables, and the constructor code assigns them to the instance fields:
```java
this.color = col;
this.year = year;
this.make = make;
this.model = model;
```
Now the object's fields hold meaningful values.

### Step 3: Put Address of Instance into Reference Variable (Slide 17)
The constructor completes. The `new` expression evaluates to the **memory address** (reference) of the newly created object on the heap. 

Java then performs the assignment (`=`), storing that address inside the local reference variable `c` on the **Call Stack**. `c` does not store the car itself—it stores the reference (pointer) to the car!

---

## Example B: The Car Class Definition (Slide 13, 14)

Consider the following `Car` class written by a student:

```java
public class Car {
    // 1. Private Instance Variables (Attributes)
    private String color;
    private int year;
    private String make;
    private String model;

    // 2. Constructor (Initializes fields)
    public Car(String col, int year, String make, String model) {
        this.color = col;
        this.year = year;
        this.make = make;
        this.model = model;
    }

    // 3. Instance Method (Getter / Accessor)
    public String getColor() {
        return color;
    }
}
```

Notice the role of each section:
* `private String color; ...`: Instance variables defining the state of any `Car`.
* `public Car(...)`: The constructor that executes during Step 2 of instantiation.
* `public String getColor()`: An instance method that returns the current color of the specific car object.

---

## Task B: Student Action — Creating & Tracing Car Objects

Medium
{: .label .label-yellow }

This task directly follows the **Student Action** specified in Slides 13 and 14:
1. Write Java statements to create a `Car` instance.
2. Write Java statements to call an instance method to get the color of the car.
3. Trace how references and object states work when multiple instances are created.

### Instructions:
1. Create a file named `Car.java` using the `Car` class from Example B as a base.
2. In `Car.java`, add the following methods:
   * A getter method `public int getYear()` to return the manufacturing year.
   * A description method `public String getDescription()` to return `year + " " + make + " " + model`.
3. Create a separate test client file named `CarClient.java` with a `main` method.
4. Inside `CarClient.java`:
   * **Action 1 (Slide 14):** Create a `Car` instance named `c1` with `"Yellow"`, `2025`, `"Tesla"`, `"Model Y"`.
   * **Action 2 (Slide 14):** Call `.getColor()` on `c1` and print the color.
   * **Action 3:** Create a second `Car` instance named `c2` with `"Red"`, `2024`, `"Ford"`, `"Mustang"`.
   * **Action 4:** Create a third variable: `Car c3 = c1;` (Assigning the reference, NOT creating a new object!).
   * **Action 5:** Print the descriptions of `c1` and `c2` using `.getDescription()`.
   * **Action 6:** Compare references: print whether `(c1 == c2)` is true/false, and whether `(c1 == c3)` is true/false.

### Code Skeleton for `Car.java`:
```java
public class Car {
    private String color;
    private int year;
    private String make;
    private String model;

    public Car(String col, int year, String make, String model) {
        this.color = col;
        this.year = year;
        this.make = make;
        this.model = model;
    }

    public String getColor() {
        return color;
    }

    // TODO 1: Implement getYear() to return the year
    public int getYear() {
        // Your code here
        return 0;
    }

    // TODO 2: Implement getDescription() to return year + " " + make + " " + model
    public String getDescription() {
        // Your code here
        return "";
    }
}
```

### Code Skeleton for `CarClient.java`:
```java
public class CarClient {
    public static void main(String[] args) {
        // TODO 1: Create first Car instance c1 (Slide 14 Student Action: "Yellow", 2025, "Tesla", "Model Y")
        

        // TODO 2: Call instance method to get and print the color of c1 (Slide 14 Student Action)
        

        // TODO 3: Create second Car instance c2 ("Red", 2024, "Ford", "Mustang") and print its color
        

        // TODO 4: Demonstrate reference aliasing: assign c1 to a new reference variable c3
        

        // TODO 5: Print descriptions for c1 and c2 using getDescription()
        

        // TODO 6: Print reference equality checks (c1 == c2) and (c1 == c3)
        
    }
}
```

### Expected Output:
```
Car 1 color: Yellow
Car 2 color: Red
Car 1 description: 2025 Tesla Model Y
Car 2 description: 2024 Ford Mustang
Is c1 == c2? false
Is c1 == c3? true
```

### Tracing Check (Memory Model Quiz):
Answer the following questions to verify your mental model of Slides 15–17:

1. **How many `Car` objects were created in the heap during `CarClient`'s execution?**
   * *Answer:* Exactly **2** objects were created. `c3 = c1` does **not** call `new`; it merely copies the reference address of `c1` into `c3`.
2. **Immediately after Step 1 of `new Car(...)` begins (before constructor lines run), what is the value stored in the `year` field in heap memory?**
   * *Answer:* `0` (the default value for primitive integers).
3. **What is stored directly inside the local variable `c1` on the stack?**
   * *Answer:* A 64-bit memory address (reference) pointing to the `Car` object on the heap, NOT the car data itself.


## Task C: Designing a Book Inventory (Average Price Calculator)

Medium
{: .label .label-yellow }

Now, let's practice building a complete class from scratch and performing **calculations on multiple objects**. You will create a `Book` class to represent a book in an inventory, and then write a client program to calculate the average price of a selection of books.

### Instructions:
1. Create a brand-new Java file named `Book.java`.
2. Define a class `Book` with the following **private instance variables**:
    * `String name` (the book's title)
    * `String author` (the book's author)
    * `double price` (the price of the book in USD)
    * `String isbn` (the unique ISBN code, e.g., `"978-3-16-148410-0"`)
3. Write a **constructor** with this exact signature:
   `public Book(String name, String author, double price, String isbn)`
    * It should initialize the fields with the passed arguments.
4. Write **getter methods** for all fields:
    * `public String getName()`
    * `public String getAuthor()`
    * `public double getPrice()`
    * `public String getIsbn()`
5. In `main`, execute the following scenario:
    * Construct three `Book` objects:
        1. `"The Great Gatsby"` by `"F. Scott Fitzgerald"`, price `15.99`, ISBN `"978-0-7432-7356-5"`.
        2. `"To Kill a Mockingbird"` by `"Harper Lee"`, price `12.50`, ISBN `"978-0-446-31078-9"`.
        3. `"1984"` by `"George Orwell"`, price `14.25`, ISBN `"978-0-451-52493-5"`.
    * Store these three books in an array of `Book`: `Book[] library = {book1, book2, book3};`.
    * Loop through the array, print each book's description using getters in the format `"[Title] by [Author] (ISBN: [ISBN]) - $[Price]"`, and sum up their prices.
    * Calculate and print the **average price** (round to 2 decimal places) of the books in the library.

### Expected Output:
```
The Great Gatsby by F. Scott Fitzgerald (ISBN: 978-0-7432-7356-5) - $15.99
To Kill a Mockingbird by Harper Lee (ISBN: 978-0-446-31078-9) - $12.5
1984 by George Orwell (ISBN: 978-0-451-52493-5) - $14.25

Average Book Price: $14.25
```

### Code Skeleton:
```java
public class Book {
    // TODO 1: Declare private instance variables (name, author, price, isbn)

    // TODO 2: Write parameterized constructor

    // TODO 3: Write getter methods

    public static void main(String[] args) {
        // TODO 4: Construct 3 book objects and store them in a Book[] array

        // TODO 5: Loop through the array, use getters to print details and sum their prices

        // TODO 6: Calculate and print the average price
    }
}
```


## AP CSA Objects & Classes Cheatsheet

### 1. Class vs. Object
* **Class:** The structural code template (Blueprint). Defines the instance variables and instance methods.
* **Object:** A concrete runtime instance allocated on the Heap with specific state values.

### 2. The 3-Step Creation Process
1. **Allocate Memory:** `new ClassName()` allocates space on Heap; instance fields receive default values (`0`, `0.0`, `false`, `null`).
2. **Execute Constructor:** Initializes fields with arguments or defaults.
3. **Store Reference:** Address of the heap object is stored into the reference variable on the stack.

### 3. Constructor Golden Rules
* Always has the **exact same name as the class**.
* Has **no return type** (writing `public void Car()` turns it into a standard method, NOT a constructor!).
* A class can have multiple **overloaded constructors**, provided their parameter lists have distinct types, counts, or order.

### 4. Encapsulation & Access Rules
* Instance variables should almost always be **`private`**.
* External classes (clients) interact with objects solely through **`public`** methods.
* Direct access to `private` fields from an external class results in a compile-time error: `field has private access in ClassName`.

---

## Wrap-Up & Summary

By completing this lab, you have mastered:
1. **The Blueprint vs. Instance Relationship:** How classes define templates while objects hold distinct state at runtime.
2. **Abstract Data Types (ADTs):** Using existing classes like `Color` and visualizing Josef Albers' optical color experiments.
3. **The Heap vs. Stack Memory Model:** How `new` allocates memory, how constructors initialize fields, and how variables store reference addresses.
4. **Custom Class Construction:** Writing classes with `private` instance fields, overloaded constructors, and public getter/setter methods.
5. **Information Hiding:** Enforcing encapsulation to safeguard data integrity against invalid operations.
