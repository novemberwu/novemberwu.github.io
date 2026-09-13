---
title: Lab 8 Object-Oriented Programming
nav_order: 4
parent: 2026 DSA
layout: page
math: mathjax
---

# Lab 8 Object-Oriented Programming
{: .no_toc}

## Goals:
{: .no_toc}
* Define the difference between primitive types and **Abstract Data Types (ADTs)**.
* Learn how to **use objects** by creating instances, calling constructors, and using getters/setters.
* Understand the roles of class members, instance variables, and **access modifiers** (`public` vs. `private`).
* Design and implement **custom classes** as blueprints for your own data types.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Introduction to Object-Oriented Programming

In Java, all programs are organized into classes. Up until now, we have mostly used Java's built-in primitive types (`int`, `double`, `boolean`, `char`) and built-in object types like `String`. 

### Primitives vs. Abstract Data Types (ADTs)

* **Primitive Types:** The set of values and operations are mapped directly to hardware machine representations and instructions.
* **Abstract Data Types (ADTs):** A custom data type whose internal representation is **hidden** from the client (the code that uses the class). The client interacts with the data type *only* through its public interface (its methods).

```
+-------------------------------------------------------------+
|                        Client Code                          |
|  (Interacts ONLY with public constructors & methods)        |
+-------------------------------------------------------------+
                              |
                              v [Public API]
+-------------------------------------------------------------+
|                         ADT / Class                         |
|  - Private Instance Variables (Hidden State)                |
|  - Public Methods (Encapsulated Behavior)                   |
+-------------------------------------------------------------+
```

To **use** an object, you need to know:
1. **The class name** (which is capitalized in Java).
2. **How to construct** a new object using the `new` keyword, which invokes a **constructor**.
3. **How to apply operations** to that object using the dot (`.`) operator to invoke its methods.

---

## Example A: Using Objects (The `Car` Class)

Easy
{: .label .label-green }

Below is a complete, runnable class called `Car`. It defines a custom data type with four private instance variables representing the car's state: `color`, `year`, `make`, and `model`. It also provides a public constructor, a getter, and a setter.

Create a file named `Car.java` in IntelliJ and run the code below:

```java
public class Car {
    // Instance variables: private to hide internal state
    private String color;
    private int year;
    private String make;
    private String model;

    // Constructor: a special method with no return type that initializes the object
    public Car(String col, int year, String make, String model) {
        this.color = col;
        this.year = year;
        this.make = make;
        this.model = model;
    }

    // Getter (Accessor): allows safe access to a private field
    public String getColor() {
        return this.color;
    }

    // Setter (Mutator): allows safe modification of a private field
    public void setColor(String col) {
        this.color = col;
    }

    public static void main(String[] args) {
        // Construct a new Car object
        Car c = new Car("Red", 2025, "Tesla", "Model Y");

        // Use getters to retrieve information
        System.out.println(c.getColor());

        // Use setters to change information
        c.setColor("Black");
        System.out.println(c.getColor());
    }
}
```

### Expected Output
```
Red
Black
```

---

## Task A: Extending the Object Client

Easy
{: .label .label-green }

Now, let's extend the `Car` class to track mileage. In the real world, a car's odometer increases as it drives, but it can never decrease. We will encapsulate this rule using an instance variable and a custom method.

### Instructions:
1. Open your `Car.java` file.
2. Add a new private instance variable: `private double mileage;`.
3. Modify the **constructor** so that every newly created car starts with `0.0` miles. (You do not need to add a mileage parameter to the constructor argument list; just initialize `this.mileage = 0.0;` inside the constructor body).
4. Write a **getter method** with this exact signature: `public double getMileage()`.
5. Write a **mutator method** named `drive` with this exact signature: `public void drive(double miles)`.
   * Inside `drive`, only add `miles` to `this.mileage` if the value of `miles` is positive (greater than `0.0`). Negative miles should be ignored!
6. Modify your `main` method to:
   * Construct a car `c` representing a `"Red"`, `2025`, `"Tesla"`, `"Model Y"`.
   * Call `c.setColor("Black");` to paint it black.
   * Call `c.drive(120.5);` to drive it on a trip.
   * Call `c.drive(45.2);` to drive it on a second trip.
   * Attempt a negative drive: `c.drive(-10.0);` (this should have no effect!).
   * Print the car's updated mileage.
   * Construct a second car `c2` representing a `"Blue"`, `2026`, `"Rivian"`, `"R1T"`.
   * Call `c2.drive(85.0);`.
   * Print `c2`'s color using `getColor()` and print its mileage.

### Expected Output:
```
Car mileage after driving: 165.7
Rivian color: Blue
Rivian mileage: 85.0
```

### Code Skeleton:
```java
public class Car {
    private String color;
    private int year;
    private String make;
    private String model;
    // TODO 1: Add private mileage variable here

    public Car(String col, int year, String make, String model) {
        this.color = col;
        this.year = year;
        this.make = make;
        this.model = model;
        // TODO 2: Initialize mileage here
    }

    public String getColor() {
        return this.color;
    }

    public void setColor(String col) {
        this.color = col;
    }

    // TODO 3: Implement getMileage getter

    // TODO 4: Implement drive(double miles) method

    public static void main(String[] args) {
        // TODO 5: Implement the main client workflow as specified
    }
}
```

---

## Example B: Creating Your Own Data Type (The `ProductReview` Class)

Medium
{: .label .label-yellow }

To create a new data type in Java, we write a class blueprint. The class specifies:
1. **Instance variables** (the data fields belonging to each object).
2. **Constructors** (how to create and initialize a new object).
3. **Instance methods** (the operations that can be performed on the object).

Below is the `ProductReview` class representing an Amazon-style review. It encapsulates details about who reviewed the product, what rating they gave, the product's ID, and their written comments.

Create a file named `ProductReview.java` and run the code below:

```java
public class ProductReview {
    // Instance variables: private for encapsulation
    private int rating;
    private String comment;
    private int productId;
    private String author;

    // Parameterized constructor
    public ProductReview(String author, int rating, int productId, String comment) {
        this.author = author;
        this.rating = rating;
        this.productId = productId;
        this.comment = comment;
    }

    // Instance method to modify the review's comment
    public void updateComment(String newComment) {
        this.comment = newComment;
    }

    // Getters for accessing private state
    public int getRating() {
        return this.rating;
    }

    public String getComment() {
        return this.comment;
    }

    public int getProductId() {
        return this.productId;
    }

    public String getAuthor() {
        return this.author;
    }

    public static void main(String[] args) {
        // Construct a product review object
        ProductReview pr = new ProductReview("Monica", 5, 222, "Just OK");
        System.out.println(pr.getAuthor() + " rated product " + pr.getProductId() + " as " + pr.getRating() + " stars.");
        System.out.println("Original Comment: " + pr.getComment());

        // Update comment using instance method
        pr.updateComment("The product is awesome");
        System.out.println("Updated Comment: " + pr.getComment());
    }
}
```

### Expected Output
```
Monica rated product 222 as 5 stars.
Original Comment: Just OK
Updated Comment: The product is awesome
```

---

## Task B: Extending Product Reviews with Image Uploads

Medium
{: .label .label-yellow }

In real-world e-commerce platforms (like Amazon or Coupang), product reviews are much more helpful when customers can **upload pictures** of the actual product they received. In fact, if you look back at **Slide 9**, you'll see a real-world review with several images. 

Let's modify our `ProductReview` class to allow uploading a picture URL!

### Instructions:
1. Open your `ProductReview.java` file.
2. Add a new private instance variable: `private String pictureUrl;`.
3. Modify the **constructor** so that by default, `pictureUrl` is initialized to the string `"No picture uploaded"`. (Note: Do not add a `pictureUrl` parameter to the constructor argument list — every new review simply starts with no picture by default).
4. Write a **getter method** with this exact signature: `public String getPictureUrl()`.
5. Write a **mutator method** named `uploadPicture` with this exact signature: `public void uploadPicture(String url)`.
   * It should update `this.pictureUrl` with the passed `url`.
6. Modify your `main` method to run this scenario:
   * Construct a product review `pr` with author `"Monica"`, rating `5`, product ID `222`, and comment `"Just OK"`.
   * Print the initial picture URL.
   * Call `pr.updateComment("The product is awesome");`.
   * Upload a picture URL: `"https://images.com/keyboard.jpg"`.
   * Print the review's updated comment and the uploaded picture URL to verify that it was successfully saved.

### Expected Output:
```
Monica rated product 222 as 5 stars.
Original Comment: Just OK
Initial Picture: No picture uploaded
Updated Comment: The product is awesome
Uploaded Picture: https://images.com/keyboard.jpg
```

### Code Skeleton:
```java
public class ProductReview {
    private int rating;
    private String comment;
    private int productId;
    private String author;
    // TODO 1: Add private pictureUrl variable here

    public ProductReview(String author, int rating, int productId, String comment) {
        this.author = author;
        this.rating = rating;
        this.productId = productId;
        this.comment = comment;
        // TODO 2: Initialize pictureUrl here
    }

    public void updateComment(String newComment) {
        this.comment = newComment;
    }

    // TODO 3: Implement getPictureUrl getter

    // TODO 4: Implement uploadPicture(String url) mutator method

    public int getRating() { return this.rating; }
    public String getComment() { return this.comment; }
    public int getProductId() { return this.productId; }
    public String getAuthor() { return this.author; }

    public static void main(String[] args) {
        // TODO 5: Complete the client workflow as specified in the instructions
    }
}
```

---

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

---

## Wrap-Up: Object-Oriented Principles in Action

By completing this lab, you have implemented **encapsulation** (hiding fields using `private` and exposing them only via `public` methods) and designed **custom data types** that map perfectly to real-world entities like `Car` and `ProductReview`.

As we transition into writing core data structures (like LinkedLists, Stacks, Queues, and Trees), you will use these exact object-oriented principles:
* Each data structure will be represented by a **Class**.
* Elements in the data structures will be represented by helper objects (like a `Node` object that stores data and references to other nodes).
* Your structures will protect their internal arrays or pointers from the outside world using the `private` modifier, exposing only safe, public methods like `push()`, `pop()`, `insert()`, and `remove()`.
