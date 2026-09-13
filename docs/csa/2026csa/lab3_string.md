---
title: Lab 3 String
layout: page
nav_order: 3
parent: 2026csa
math: mathjax
---

# Lab 3: Strings
{: .no_toc }

## Goals
{: .no_toc}
* Understand that `String` is a **reference type** and how it differs from primitive types.
* Learn how to declare, initialize, and reassign `String` variables.
* Master the rules of **String concatenation** using the `+` operator.
* Understand the remote control analogy: how reference variables point to objects in memory.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Declaring and Initializing Strings

A **String** is a sequence of characters enclosed in double quotes (Slide 31).

```java
String school = "CS Academy";
String ss = new String("CS Academy");
```

Unlike primitive types (`int`, `double`, `boolean`), `String` is a **reference type**. This means a String variable does not hold the actual letters inside itself. Instead, it holds a reference (a memory address) pointing to where the letters reside in memory.

---

## Example A: Declaring and Concatenating Strings

Easy
{: .label .label-green }

The `+` operator is used to **concatenate** (join) two or more Strings together into one longer String (Slide 29).

Create a file named `StringBasics.java` in IntelliJ and run the code below:

```java
public class StringBasics {
    public static void main(String[] args) {
        // Declare and initialize String variables
        String greeting = "hello";
        String name = "Rachel";

        // Join strings together using the + operator
        String message = greeting + ", " + name;

        // Print the concatenated result
        System.out.println(message);
    }
}
```

### Expected Output:
```
hello, Rachel
```

---

## Task A: Your First String Concatenation

Easy
{: .label .label-green }

Let's practice declaring String variables and joining them together.

### Instructions:
1. Create a Java file named `StringPractice.java`.
2. Inside `main`, declare two String variables:
   * `firstName` initialized to your first name (e.g., `"Rachel"`).
   * `lastName` initialized to your last name (e.g., `"Wu"`).
3. Declare a third String variable named `fullName`.
4. Use the `+` operator to combine `firstName`, a space `" "`, and `lastName` into `fullName`.
5. Print the value of `fullName` to the console.

### Expected Output (using Rachel Wu as an example):
```
Rachel Wu
```

### Code Skeleton:
```java
public class StringPractice {
    public static void main(String[] args) {
        // TODO 1: Declare firstName and lastName variables

        // TODO 2: Combine them with a space using the + operator
        String fullName = ""; // Update this

        // TODO 3: Print the fullName variable
    }
}
```

---

## Part 2: Mixing Strings and Numbers

The `+` operator is very powerful. When you combine a `String` and a number (such as an `int` or a `double`), Java automatically converts the number into text and joins them together (Slide 29).

However, remember that **order matters**:
* `"1" + "2"` evaluates to the text `"12"`, NOT the number `3`.
* `1 + 2` evaluates to the number `3`.

---

## Example B: Concatenating Strings and Numbers

Easy
{: .label .label-green }

Let's observe how Java handles numbers when they are treated as text.

Create a file named `StringNumbers.java` and run the code below:

```java
public class StringNumbers {
    public static void main(String[] args) {
        String s1 = "10";
        String s2 = "20";

        // Since s1 and s2 are Strings, + joins them as text
        String joinedText = s1 + s2;
        System.out.println("Joined: " + joinedText);

        // Standard integer addition
        int a = 10;
        int b = 20;
        int sum = a + b;
        System.out.println("Sum: " + sum);
    }
}
```

### Expected Output:
```
Joined: 1020
Sum: 30
```

---
## Task B: Generating User Profile

Easy
{: .label .label-green }

Write a program named `ProfileGenerator.java` that constructs a formatted user profile using String concatenation. The input values are defined directly as variables inside your `main` method.

{: .note }
Do not use any String methods (like `.substring()`, `.length()`, etc.) for this task. Use only the `+` operator for concatenation.

### Instructions:
1. Create a class named `ProfileGenerator` with a `main` method.
2. Inside `main`, use the predefined variables:
   * `String firstName = "Alice";`
   * `String lastName = "Smith";`
   * `int birthYear = 2008;`
3. Construct and print two messages using String concatenation (`+`):
   * A profile creation message: `"Profile created for [firstName] [lastName] [birthYear]"`
   * A welcome message: `"Welcome, [firstName] [lastName]!"`

### Expected Output
```
Profile created for Alice Smith 2008
Welcome, Alice Smith!
```

### Code Skeleton:
```java
public class ProfileGenerator {
    public static void main(String[] args) {
        // Predefined user variables
        String firstName = "Alice";
        String lastName = "Smith";
        int birthYear = 2008;

        // TODO 1: Construct the profile message using only the variables above and the + operator
        String profileMessage = ""; // Update this

        // TODO 2: Construct the welcome message
        String welcomeMessage = ""; // Update this

        // TODO 3: Print both messages
    }
}
```

---

## Part 3: The Reference Model (The "Remote Control" Analogy)

Because `String` is a reference type, a String variable acts like a **remote control** pointing to a String object in memory (Slide 33).

When you assign one reference variable to another:
```java
String t = s;
```
You are **not copying the actual text**. Instead, you are making a copy of the "remote control." Both variables (`s` and `t`) now point to the exact same String object in memory.

---

## Example C: String equality

Easy
{: .label .label-green }

To compare the equality of two String objects' content, use the `.equals()` method, not the `==` operator.

Observe the following statements on your machine, and see if they match your expectation:

```java
public class StringEqualsExamples {

    public static void main(String[] args){
        String s1 = "hello"; // Stored in the String Pool
        String s2 = "hello"; // Refers to the same object in the String Pool
        String s3 = new String("hello"); // Creates a new object in the heap
        String s4 = new String("hello");

        System.out.println("s1 == s2: " + (s1 == s2)); // true (same object in String Pool)
        System.out.println("s1 == s3: " + (s1 == s3)); // false (different objects in memory)
        System.out.println("s3 == s4: " + (s3 == s4)); // false (different objects in memory)

        System.out.println("s1 equals s2: " + (s1.equals(s2))); // true (same content)
        System.out.println("s1 equals s3: " + (s1.equals(s3))); // true (same content)
        System.out.println("s3 equals s4: " + (s3.equals(s4))); // true (same content)
    }
}
```

---

## Task C: Login Status Matcher

Easy
{: .label .label-green }

Write a program named `LoginStatus.java` that evaluates if a user's input matches the system administrator's credentials. The inputs are represented by variables defined directly inside your `main` method.

Without using any `if` statements or complex String methods, evaluate if the credentials match using `.equals()` and the logical AND (`&&`) operator.

{: .note }
To compare String content, you must use `.equals()`. Do not use `==` for String content comparison.

### Instructions:
1. Create a class named `LoginStatus` with a `main` method.
2. Inside `main`, use the predefined variables representing system credentials and user inputs:
   * `String storedUsername = "admin";`
   * `String storedPassword = "secret123";`
   * `String inputUsername = "admin";`
   * `String inputPassword = "secret123";`
3. Perform a Boolean comparison:
   * Compare `inputUsername` with `storedUsername` using `.equals()`.
   * Compare `inputPassword` with `storedPassword` using `.equals()`.
   * Combine them using the `&&` operator to verify both match.
4. Print the result exactly in the format: `"Login authorized: [true/false]"`.
5. Try changing `inputUsername = "guest";` in your code to verify that the output correctly updates to `false`.

### Expected Output
```
Login authorized: true
```

### Code Skeleton:
```java
public class LoginStatus {
    public static void main(String[] args) {
        // System administrator credentials
        String storedUsername = "admin";
        String storedPassword = "secret123";

        // Simulated user inputs (you can modify these to test!)
        String inputUsername = "admin";
        String inputPassword = "secret123";

        // TODO 1: Evaluate if credentials match using .equals() and the logical AND (&&) operator
        // Do not use any 'if' statements or complex methods!
        boolean isAuthorized = false; // Update this

        // TODO 2: Print the authorization result in the format: "Login authorized: <true/false>"
    }
}
```


---

## AP CSA String Cheatsheet

Here is a quick-reference guide of the essential `String` rules tested on the **AP Computer Science A** exam for beginners.

### References vs. Primitives
* **Primitive Types (`int`, `double`, `boolean`):** The variable holds the **actual value** directly inside itself.
* **Reference Types (`String`):** The variable holds a **reference (memory address)** pointing to where the object is stored. It acts like a remote control pointing to a television.

### Golden Rules of String Concatenation (`+`)
1. **Joining Text:** Joining two Strings creates a brand-new String containing the characters of both.
2. **Text + Number:** Adding any number to a String (e.g., `"Age: " + 16`) converts the number to text and joins them (results in `"Age: 16"`).
3. **Left-to-Right Evaluation:** Java evaluates addition and concatenation from left to right.
   * `1 + 2 + "3"` evaluates to `"33"` (adds `1 + 2` first to get `3`, then concatenates with `"3"`).
   * `"1" + 2 + 3` evaluates to `"123"` (concatenates `"1"` and `2` first to get `"12"`, then concatenates with `3`).

### What Lies Ahead: String Methods
In our upcoming labs, we will learn how to use the "remote control" to call **built-in methods** on String objects, such as:
* Checking string length using `.length()`
* Extracting sections of text using `.substring(...)`
* Searching for characters using `.indexOf(...)`
* Comparing text contents using `.equals(...)`

For now, master the art of declaring, concatenating, and reassigning references!

---

## Wrap-Up: Building a Core Mental Model of Reference Types

By completing this lab, you have gained vital insights into how reference types work in Java:
* Variables for reference types store **addresses (references)** rather than actual data.
* Creating new assignments like `copy = original` duplicates the pointer/reference, not the actual object.
* Reassigning one reference variable has no effect on other variables pointing to the original object.
