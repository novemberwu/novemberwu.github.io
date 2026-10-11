---
title: Lab 9 Creating Classes
layout: page
nav_order: 9
parent: 2026csa
math: mathjax


---

## Part 1: Creating Data Types: Instance Variables & Constructors

When designing your own custom classes (Slide 18, 19), a Java class typically contains four distinct components:
1. **Instance Variables:** Define the set of values (state) each object maintains.
2. **Constructors:** Procedures that initialize newly allocated objects.
3. **Instance Methods:** Operations that read or modify the object's state.
4. **Test Client (`main`):** Code that instantiates and tests the objects.

```
+-------------------------------------------------------+
|  public class ClassName                               |
|  +-------------------------------------------------+  |
|  |  1. Instance Variables                          |  |
|  |     private datatype name;                      |  |
|  +-------------------------------------------------+  |
|  |  2. Constructors                                |  |
|  |     public ClassName(...) { ... }               |  |
|  +-------------------------------------------------+  |
|  |  3. Instance Methods                            |  |
|  |     public returnType methodName(...) { ... }   |  |
|  +-------------------------------------------------+  |
|  |  4. Test Client (main method)                   |  |
|  |     public static void main(String[] args)      |  |
|  +-------------------------------------------------+  |
+-------------------------------------------------------+
```

### Instance Variable Syntax (Slide 20, 21)
```java
private [static] [final] datatype name;
```
* **`private`:** Access modifier. Encapsulates the field so outside classes cannot directly manipulate it.
* **`[static]` (Optional):** Means the field belongs to the class itself and is shared across all instances. (Without `static`, every object gets its own independent copy).
* **`[final]` (Optional):** Makes the variable a constant (it cannot be reassigned after initialization).
* **`datatype`:** The type of data stored (primitives like `int`, `double`, `boolean`, or reference types like `String`, `Color`).
* **`name`:** Identifier for the field (written in `camelCase`).

### What is a Constructor? (Slide 24, 25)
* Short procedures used for creating and initializing objects of a class.
* **Always have the exact same name as the class** (including capitalization).
* **Do NOT have a return type** — not even `void`!
* Initialize the object's instance fields.
* May take parameters to supply initial values.
* **Constructor Overloading:** A class may define multiple constructors, as long as each constructor has a unique parameter list (differing in the number, types, or order of parameters).

---

## Example A: Amazon Product Review (Slide 22, 23, 26)

In modern e-commerce systems like Amazon, customer feedback is modeled as a data type (Slide 22). What attributes define an Amazon product review?
* `productId`: ID of the item being reviewed (`int`)
* `rating`: Star rating from 1 to 5 (`int`)
* `comment`: Written feedback text (`String`)
* `author`: Customer's username (`String`)
* `images`: Attached photos (`String[]`)
* `videos`: Attached video clips (`String[]`)

Here is the implementation of the `ProductReview` class with **overloaded constructors** (Slide 23, 26):

```java
public class ProductReview {
    // Private Instance Variables (Slide 21, 23)
    private int rating;
    private String comment;
    private int productId;
    private String author;
    private String[] images;
    private String[] videos;

    // Constructor 1: 3-Parameter Constructor (For quick ratings without text comment)
    public ProductReview(String author, int rating, int productId) {
        this.author = author;
        this.rating = rating;
        this.productId = productId;
        this.comment = ""; // Default empty comment
    }

    // Constructor 2: 4-Parameter Constructor (Overloaded: includes a written comment)
    public ProductReview(String author, int rating, int productId, String comment) {
        this.author = author;
        this.rating = rating;
        this.productId = productId;
        this.comment = comment;
    }

    // Test Client (Slide 26)
    public static void main(String[] args) {
        // Invoking Constructor 1 (Slide 26)
        ProductReview pr = new ProductReview("Monica", 5, 222);
        System.out.println("Created review by " + pr.author + " with rating " + pr.rating);
    }
}
```

Notice how Java distinguishes between the two constructors: when you call `new ProductReview("Monica", 5, 222)`, Java matches the arguments against the 3-parameter constructor signature `(String, int, int)`.

---

## Task A: Constructor Overloading Detective

Medium
{: .label .label-yellow }

Let's expand the `ProductReview` class to master constructor overloading and field initialization.

### Instructions:
1. Create a file named `ProductReview.java`.
2. Add the private fields: `author`, `rating`, `productId`, and `comment`.
3. Implement three overloaded constructors:
    * **Constructor 1:** `(String author, int rating, int productId)` — initializes author, rating, productId, and sets `comment = "No written review"`.
    * **Constructor 2:** `(String author, int rating, int productId, String comment)` — initializes all four fields.
    * **Constructor 3 (Default Star Constructor):** `(String author, int productId)` — for customers who click "1-click 5-star rating", initializes author, productId, automatically sets `rating = 5`, and sets `comment = "Default 5-star rating"`.
4. Add a public method `public void displaySummary()` that prints the review's author, product ID, rating, and comment.
5. In the `main` method, instantiate three reviews, each using a different constructor, and display their summaries.

### Code Skeleton:
```java
public class ProductReview {
    private int rating;
    private String comment;
    private int productId;
    private String author;

    // Constructor 1 (3 parameters)
    public ProductReview(String author, int rating, int productId) {
        this.author = author;
        this.rating = rating;
        this.productId = productId;
        this.comment = "No written review";
    }

    // Constructor 2 (4 parameters)
    public ProductReview(String author, int rating, int productId, String comment) {
        this.author = author;
        this.rating = rating;
        this.productId = productId;
        this.comment = comment;
    }

    // TODO 1: Implement Constructor 3 (2 parameters: author, productId)
    // Sets this.author and this.productId, defaults rating to 5, and comment to "Default 5-star rating"
    public ProductReview(String author, int productId) {
        // Your code here
    }

    // TODO 2: Implement displaySummary() to print:
    // "[<author> | Item #<productId>] <rating> Stars: \"<comment>\""
    public void displaySummary() {
        // Your code here
    }

    public static void main(String[] args) {
        // TODO 3: Construct r1 using 3 parameters ("Monica", 5, 222)
        

        // TODO 4: Construct r2 using 4 parameters ("Alex", 4, 101, "Great keyboard, slightly loud keys.")
        

        // TODO 5: Construct r3 using 2 parameters ("Jordan", 305)
        

        // TODO 6: Call displaySummary() on r1, r2, and r3 to verify output
        
    }
}
```

### Expected Output:
```
[Monica | Item #222] 5 Stars: "No written review"
[Alex | Item #101] 4 Stars: "Great keyboard, slightly loud keys."
[Jordan | Item #305] 5 Stars: "Default 5-star rating"
```

---

## Part 2: Instance Methods, Encapsulation & Access Control

### 1. Encapsulation and Information Hiding (Slide 28)
**Encapsulation** is a cornerstone principle of software engineering:
* A class bundles data (fields) and methods together, and **hides the internal implementation details** from external code.
* External classes interact with an object **only through public constructors and public methods** (Slide 28).
* Why is encapsulation essential?
    * **Integrity:** Prevents outside code from corrupting an object's internal state (e.g., setting a star rating to `-99`).
    * **Maintainability:** You can change how data is stored internally (e.g., changing an array to an `ArrayList`) without breaking any client programs that use the class.
    * **Team Collaboration:** Different engineers can work on different classes simultaneously by agreeing upon the public method signatures.

### 2. Method Invocations on Objects (Slide 29)
* You invoke an instance method for a specific object using the dot operator:
  ```java
  pr.setRating(4);
  ```
* The number and types of arguments passed in **must match** the parameters defined in the method signature (Slide 29).

### 3. Access Boundaries: Class X vs. Class Y (Slide 30)
Slide 30 illustrates the strict access rules between classes:

```
        Class X (Definition)                      Class Y (External Client)
   +-------------------------------+          +-------------------------------+
   | private field                 | <---X--- | Attempts direct field access  | (FORBIDDEN: Compiler Error)
   |                               |          |                               |
   | public method (e.g. setRating)| <======= | Calls public method           | (ALLOWED: Public API)
   |       │                       |          |                               |
   |       ▼                       |          |                               |
   | private method (helper)       | <---X--- | Attempts to call helper method| (FORBIDDEN: Compiler Error)
   +-------------------------------+          +-------------------------------+
```

* **Within the same class (`Class X`):** Constructors and methods can freely access all public and private fields and methods.
* **From another class (`Class Y`):** Code can **only** call `public` methods and constructors. Direct access to `private` fields or `private` methods is blocked by the Java compiler!

---

## Example B: Mutator Methods & Encapsulation Defense (Slide 29, 30)

Let's see what happens when we add a mutator method (`setRating`) with validation logic inside `ProductReview.java`:

```java
public class ProductReview {
    private int rating;
    private String comment;
    private int productId;
    private String author;

    public ProductReview(String author, int rating, int productId) {
        this.author = author;
        this.productId = productId;
        setRating(rating); // Reuses setRating to enforce validation!
    }

    // Public Mutator Method (Setter) with boundary validation (Slide 29)
    public void setRating(int newRating) {
        if (newRating >= 1 && newRating <= 5) {
            this.rating = newRating;
        } else {
            System.out.println("Warning: Invalid rating (" + newRating + "). Must be between 1 and 5.");
        }
    }

    // Public Accessor Method (Getter)
    public int getRating() {
        return this.rating;
    }
}
```

Now, consider an external test class `Client.java` (Class Y):

```java
public class Client {
    public static void main(String[] args) {
        ProductReview pr = new ProductReview("Monica", 5, 222);

        // Valid method call (Slide 29)
        pr.setRating(4); // Updates rating to 4 safely
        System.out.println("Updated rating: " + pr.getRating());

        // ILLEGAL ACCESS (Slide 30):
        // pr.rating = 100; // COMPILER ERROR: rating has private access in ProductReview!
        
        // Invalid value filtered safely:
        pr.setRating(100); // Filtered out by the validation logic in setRating()
    }
}
```

Because `rating` is `private`, client code cannot bypass our rules and set an absurd rating like `100`. This is the power of **Encapsulation**!

---

## Task B: Complete E-Commerce Review System

Hard
{: .label .label-red }

In this task, you will put all the pieces together: private instance variables, overloaded constructors, public accessor/mutator methods with input validation, helper methods, and access control testing.

### Requirements:
1. Create a class file named `ProductReview.java` containing:
    * Private fields: `author` (`String`), `productId` (`int`), `rating` (`int`), and `comment` (`String`).
    * Overloaded Constructors:
        * `public ProductReview(String author, int rating, int productId)`
        * `public ProductReview(String author, int rating, int productId, String comment)`
    * Public Getters:
        * `getAuthor()`, `getProductId()`, `getRating()`, `getComment()`
    * Public Mutators:
        * `public void setRating(int newRating)`: Validates that `newRating` is between 1 and 5 (inclusive). If invalid, print `"Error: Rating must be 1-5."` and leave `rating` unchanged.
        * `public void updateComment(String newComment)`: Updates the review's text comment.
    * Public Utility Methods:
        * `public boolean isPositive()`: Returns `true` if `rating >= 4`, otherwise `false`.
        * `public void printReview()`: Nicely formats and prints all review details.
2. Create a separate tester class named `ReviewTester.java` in the same directory:
    * Instantiate at least two reviews.
    * Test updating the rating with both a valid rating (`4`) and an invalid rating (`7`).
    * Verify that `isPositive()` returns the expected boolean.
    * **Deliberately test access control:** Write a commented-out line attempting to access `review.rating = 5;` and confirm that uncommenting it results in a compilation error: `rating has private access in ProductReview`.

### Code Skeleton for `ProductReview.java`:
```java
public class ProductReview {
    private String author;
    private int productId;
    private int rating;
    private String comment;

    // TODO 1: Implement 3-parameter constructor (author, rating, productId)
    // Sets fields, defaults comment to "", and uses setRating(rating) to enforce validation
    public ProductReview(String author, int rating, int productId) {
        // Your code here
    }

    // TODO 2: Implement 4-parameter constructor (author, rating, productId, comment)
    // Sets fields, and uses setRating(rating) to enforce validation
    public ProductReview(String author, int rating, int productId, String comment) {
        // Your code here
    }

    // Getters
    public String getAuthor() { return author; }
    public int getProductId() { return productId; }
    public int getRating() { return rating; }
    public String getComment() { return comment; }

    // TODO 3: Implement setRating(int newRating)
    // Validate that newRating is between 1 and 5 (inclusive).
    // If valid, assign to this.rating. If invalid, print "Error: Rating must be 1-5."
    public void setRating(int newRating) {
        // Your code here
    }

    // TODO 4: Implement updateComment(String newComment)
    public void updateComment(String newComment) {
        // Your code here
    }

    // TODO 5: Implement isPositive() - returns true if rating >= 4, false otherwise
    public boolean isPositive() {
        // Your code here
        return false;
    }

    // TODO 6: Implement printReview() to display formatted review details
    public void printReview() {
        // Your code here
    }
}
```

### Code Skeleton for `ReviewTester.java`:
```java
public class ReviewTester {
    public static void main(String[] args) {
        // TODO 1: Create two reviews using overloaded constructors
        // r1: "SarahT", 5 stars, product 8801, comment "Outstanding noise cancellation!"
        // r2: "Dave99", 2 stars, product 8801, no comment
        

        // TODO 2: Print both reviews using printReview()
        

        // TODO 3: Update Dave99's review to 3 stars and update comment to
        // "Customer support sent a replacement, works better now.", then print again
        

        // TODO 4: Attempt invalid rating update (6 stars) and verify error message
        

        // TODO 5: Demonstrate encapsulation boundary:
        // Add a commented-out line attempting to access r2.rating directly:
        // r2.rating = 5; // ERROR: rating has private access in ProductReview
        
    }
}
```

### Expected Output:
```
----------------------------------------
Product ID: 8801 | Reviewer: SarahT
Rating:     5 / 5 Stars (Positive)
Comment:    Outstanding noise cancellation!
----------------------------------------
----------------------------------------
Product ID: 8801 | Reviewer: Dave99
Rating:     2 / 5 Stars (Critical)
Comment:    [No comment provided]
----------------------------------------

Updating Dave99's review...
----------------------------------------
Product ID: 8801 | Reviewer: Dave99
Rating:     3 / 5 Stars (Critical)
Comment:    Customer support sent a replacement, works better now.
----------------------------------------

Attempting invalid rating update (6 stars)...
Error: Rating must be 1-5.
```

---