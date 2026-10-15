---
title: Lab 10 String Manipulation
layout: page
nav_order: 10
parent: 2026csa
math: mathjax
---

# Lab 10: String Manipulation & The String ADT
{: .no_toc }

## Goals
{: .no_toc}
* Understand that `String` is an **Abstract Data Type (ADT)**.
* Master 0-based character indexing and index boundaries in Java.
* Apply core AP CSA `String` methods: `length()`, `indexOf()`, and `substring()`.
* Internalize **String immutability**.
* Learn how to **override `toString()`** using `StringBuilder`.
* Build a clean `UserName` generator class aligned with the AP Classroom format.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: String ADT & Indexing (Slide 2, 3)

A `String` is an **Abstract Data Type (ADT)** representing a sequence of Unicode characters (Slide 3). Its internal representation is encapsulated and hidden.

Characters in a String use **0-based indexing** (Slide 7, 9):

```
String s = "Java";

 Character:  | 'J' | 'a' | 'v' | 'a' |
 Index:      |  0  |  1  |  2  |  3  |

 First index: 0
 Last index:  s.length() - 1  ==>  3
```

---

## Part 2: Core String Methods (Slide 5–10)

```
public class String
-------------------------------------------------------------------------------------------------
int length()                         // Total character count
int indexOf(String str)              // Index of first occurrence, or -1 if not found
int indexOf(String str, int from)    // Index of first occurrence starting from 'from'
String substring(int begin, int end) // Substring from begin (inclusive) to end (exclusive)
String substring(int begin)          // Substring from begin to the end of string
boolean equals(Object other)         // True if characters match exactly
```

---

## Example A: String Methods in Action (Slide 6, 8, 10)

```java
public class StringDemo {
    public static void main(String[] args) {
        String s = "Java";

        // 1. length() (Slide 5, 6)
        System.out.println("Length: " + s.length()); // 4

        // 2. indexOf() (Slide 7, 8)
        System.out.println("First 'a': " + s.indexOf('a'));     // 1
        System.out.println("Next 'a': " + s.indexOf('a', 2));   // 3
        System.out.println("Find \"av\": " + s.indexOf("av"));  // 1

        // 3. substring() [inclusive, exclusive) (Slide 9, 10)
        System.out.println("s.substring(1, 3): " + s.substring(1, 3)); // "av"
        System.out.println("s.substring(2): " + s.substring(2));       // "va"
    }
}
```

---

## Task A: Word Separator (Slide 11)

Easy
{: .label .label-green }

Given the String `"Java Programming"`, print out the 2 words on separate lines (Slide 11). Use `indexOf` to find the space dynamically, and use `substring` to extract each word.

### Instructions:
1. Create a file named `WordSeparator.java`.
2. Find the index of the space using `phrase.indexOf(" ")`.
3. Extract `word1` (from index `0` up to `space`) and `word2` (from `space + 1` to the end).
4. Print each word on a new line.

### Code Skeleton:
```java
public class WordSeparator {
    public static void main(String[] args) {
        String phrase = "Java Programming";

        // TODO 1: Find space index
        int space = phrase.indexOf(" ");

        // TODO 2: Extract both words using substring
        String word1 = phrase.substring(0, space);
        String word2 = phrase.substring(space + 1);

        System.out.println("Word 1: " + word1);
        System.out.println("Word 2: " + word2);
    }
}
```

### Expected Output:
```
Word 1: Java
Word 2: Programming
```

---

## Task B: Email Address Parser

Medium
{: .label .label-yellow }

Now let's practice parsing more complex structured text using `indexOf` and `substring`. Given an email address:

```java
String email = "rachel.wu @hotmail.com";
```

Decompose the email step by step:
1. Find and print the index of the `'@'` character.
2. Extract the **username** (the part before `'@'`).
3. Extract the **domain** (the part after `'@'`).
4. If the username follows the `<first name>.<last name>` format (e.g., `"rachel.wu"`), locate the `.` and extract the **first name** and **last name** respectively.

### Instructions:
1. Create a class named `Main` (in `Main.java`) or `EmailParser`.
2. Locate the `@` symbol using `email.indexOf('@')` and print the resulting index.
3. Extract the `username` from index `0` up to `index` using `substring`.
4. Extract the `domain` starting from `index + 1` to the end of the string.
5. In `username`, find the dot using `username.indexOf('.')`.
6. Extract `firstName` (from `0` to `dotIndex`) and `lastName` (from `dotIndex + 1`).
7. Print all extracted values to the console.

> **Note on Whitespace:**
> * If `email = "rachel.wu@hotmail.com"` (without a space), `indexOf('@')` is `9`.
> * If `email = "rachel.wu @hotmail.com"` (with a space before `@`), `indexOf('@')` is `10`, and the extracted username will be `"rachel.wu "`. Either input is great for practicing how indices and boundary limits work!

### Code Skeleton:
```java
public class Main {
    public static void main(String[] args) {
        String email = "rachel.wu @hotmail.com";

        int index = email.indexOf('@');

        System.out.println(index);

        // TODO 1: get the username from email address

        // TODO 2: get the domain from email address

        // TODO 3: if username follows the <first name>.<last name>,
        // get the first name and last name respectively
    }
}
```

### Expected Output:
*(Using `"rachel.wu@hotmail.com"`)*
```
9
Username: rachel.wu
Domain: hotmail.com
First name: rachel
Last name: wu
```

*(Or if using `"rachel.wu @hotmail.com"`)*
```
10
Username: rachel.wu 
Domain: hotmail.com
First name: rachel
Last name: wu 
```

---

## Part 3: String Immutability (Slide 12)

Strings are **immutable**—their contents can **never be changed** once created. Any string method that seems to modify text actually creates and returns a brand-new `String` object.

```java
String s = "hello";
s.toUpperCase();       // Ignored! s is NOT modified.
System.out.println(s); // Still prints "hello"

s = s.toUpperCase();   // Reassignment updates the reference to the new String
System.out.println(s); // Prints "HELLO"
```

---

## Part 4: Custom `toString()` with `StringBuilder` (Slide 13–15)

Every object in Java inherits `toString()` from `Object`. By default, printing an object outputs something like `Car@4617c264`.

To provide a readable description, override `toString()` in your class (Slide 14, 15):

```java
public class Car {
    private String color;
    private int year;
    private String make;
    private String model;

    public Car(String color, int year, String make, String model) {
        this.color = color;
        this.year = year;
        this.make = make;
        this.model = model;
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append("\ncolor: ").append(color);
        sb.append("\nyear: ").append(year);
        sb.append("\nmake: ").append(make);
        sb.append("\nmodel: ").append(model);
        return sb.toString();
    }

    public static void main(String[] args) {
        Car car = new Car("Yellow", 2025, "Tesla", "Model Y");
        System.out.println(car); // Automatically invokes car.toString()
    }
}
```

### Output:
```
color: Yellow
year: 2025
make: Tesla
model: Model Y
```

---

## Task C (Optional): Simple Palindrome Checker

Optional
{: .label .label-purple }

*(Optional: for extra practice with `substring` and loops).*

A palindrome reads the same forward and backward (e.g., `"racecar"`).

### Instructions:
1. Create `PalindromeChecker.java`.
2. In `isPalindrome(String s)`, compare characters from both ends moving inward using `substring`.
3. Return `true` if all characters match, `false` otherwise.

### Code:
```java
public class PalindromeChecker {
    public static boolean isPalindrome(String s) {
        s = s.toLowerCase();
        for (int i = 0; i < s.length() / 2; i++) {
            String front = s.substring(i, i + 1);
            String back = s.substring(s.length() - 1 - i, s.length() - i);
            if (!front.equals(back)) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println("racecar: " + isPalindrome("racecar")); // true
        System.out.println("java: " + isPalindrome("java"));       // false
        System.out.println("madam: " + isPalindrome("madam"));     // true
    }
}
```



## Homework Assignment (Slide 16)

{: .note }
> **AP Classroom: Username creation part a - e**
> * Log into [AP Classroom](https://myap.collegeboard.org).
> * Complete the assigned FRQ question **Username Creation (Parts a through e)**.
> * **Due Date:** October 27, 2026.
