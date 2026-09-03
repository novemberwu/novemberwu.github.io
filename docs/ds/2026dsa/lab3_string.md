---
title: Lab 3
nav_order: 2
parent: 2026 DSA
layout: page
math: mathjax


---
# Lab 3 String
{: .no_toc}

## Goals:
{: .no_toc}
* Able to create and manipulate String objects.
* Understand String concatenation and implicit conversion.
* Understand the difference between object reference equality (`==`) and content equality (`.equals()`).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Example A: String demo

Easy
{: .label .label-green }

String and operator
* Create a new String using String literal.
* Create a new String using the `new` keyword.
* Concatenate a String with another String.
* Concatenate a String with an integer.

```java
public class StringDemo {
    public static void main(String[] args){

        String s = "This is a String";
        
        String t = new String("This is a another String");
        System.out.println(t);

        s = s + "Hello";
        System.out.println(s);

        s = s + 9; // implicit cast from integer 9 to String "9"

        s = "99";
        int a = Integer.valueOf(s);
        System.out.println(s);
    }
}
```

---

## Task A: User Profile Generator

Easy
{: .label .label-green }

Write a program `ProfileGenerator` that takes three command-line arguments:
1. `firstName` (String)
2. `lastName` (String)
3. `birthYear` (String, which you should convert to an integer)

Your program should compute the user's approximate age (assume the current year is 2026) and print out a formatted profile message using String concatenation.

{: .note }
Do not use any String methods (like `.substring()`, `.length()`, etc.) for this task. Use only the `+` operator for concatenation.

### Sample Input
```bash
java ProfileGenerator Alice Smith 2008
```

### Sample Output
```
Profile created for Alice Smith.
Age: 18
Welcome, Alice Smith!
```

---

## Example B: String equality

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

## Task B: Login Status Matcher

Easy
{: .label .label-green }

Write a program `LoginStatus` that takes two command-line arguments: a `username` and a `password`. 

Without using any `if` statements or complex String methods, evaluate if the credentials match the system administrator's credentials:
* The required username is `"admin"`.
* The required password is `"secret123"`.

The program should print whether the login is authorized by outputting a boolean value (`true` or `false`).

{: .note }
To compare the username and password, you must use `.equals()`. Do not use `==` for String content comparison.

### Sample Input
```bash
java LoginStatus admin secret123
java LoginStatus guest secret123
```

### Sample Output
```
Login authorized: true
Login authorized: false
```
