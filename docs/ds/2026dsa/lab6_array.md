---
title: Lab 6
nav_order: 2
parent: 2026 DSA
layout: page
math: mathjax


---
# Lab 6 Arrays
{: .no_toc}

## Goals:
{: .no_toc}
* Learn how to declare, initialize, and traverse arrays in Java.
* Understand zero-based indexing and the `.length` property of arrays.
* Implement a standard array algorithm: finding the maximum value in an array.
* Implement a standard array algorithm: calculating the average of all values in an array (including casting).

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Introduction to Arrays in Java

Often in programming, we need to store a list of related items (like a list of student test scores, daily temperatures, or prices). Instead of creating 100 separate variables (`score1`, `score2`, etc.), Java allows us to store them all in a single named container called an **Array**.

An array is an ordered list of elements that all share the same data type.

### How to Declare and Initialize an Array
You can declare and initialize an array in one line using curly braces `{}`:
```java
int[] scores = {85, 92, 78, 90, 88};
```

### Accessing Elements and Zero-Based Indexing
Every item in an array is stored at a specific position called an **index**. 
* **Zero-Based Indexing:** In Java, the first element is always at index `0`. 
* **Array Length:** To get the number of elements in an array, we use `.length` (e.g. `scores.length` is `5`).
* **Last Element:** The last element of an array is always at index `.length - 1` (e.g. `scores[4]`).

```java
System.out.println(scores[0]); // Prints 85 (the first element)
System.out.println(scores[2]); // Prints 78 (the third element)
System.out.println(scores.length); // Prints 5 (the total number of elements)
```

---

## Example A: Finding the Maximum Value

Easy
{: .label .label-green }

A classic array algorithm is finding the largest (maximum) value in an array. To do this, we play the role of a referee:
1. We start by **assuming** that the very first item (at index `0`) is currently the largest, and store it in a tracking variable named `max`.
2. We use a `for` loop to look at each of the remaining items in the array (starting from index `1`).
3. For each item, we ask: *"Is this item larger than our current max?"*
4. If it is, we update our `max` to hold this new larger value.
5. Once the loop finishes checking every item, our `max` variable is guaranteed to hold the absolute largest value.

Create a file named `FindMax.java` in IntelliJ and run the code below:

```java
public class FindMax {
    public static void main(String[] args) {
        // Declare and initialize an array of integers
        int[] numbers = {12, 45, 78, 23, 56, 89, 34};

        // Step 1: Assume the first element is the maximum
        int max = numbers[0];

        // Step 2: Loop through the rest of the array starting from index 1
        for (int i = 1; i < numbers.length; i++) {
            // Step 3: If we find a larger number, update max
            if (numbers[i] > max) {
                max = numbers[i];
            }
        }

        // Step 4: Print the final maximum value found
        System.out.println("The maximum value in the array is: " + max);
    }
}
```

### Expected Output
```
The maximum value in the array is: 89
```

---

## Task A: Calculating the Average Value

Easy
{: .label .label-green }

Write a Java program named `ArrayAverage.java` that calculates the **average (mean)** value of all integers in an array.

### AP CSA Concept Alert: Double Division!
Calculating the average of integers requires special care. An average can be a decimal (e.g. the average of `2` and `3` is `2.5`). 
* If you divide the integer sum by the integer length (`sum / numbers.length`), Java will perform **integer division** and drop the decimal part!
* To get a correct decimal result, you must **cast** the sum to a `double` during division: 
  ```java
  double average = (double) sum / numbers.length;
  ```

### Instructions:
1. Create a class `ArrayAverage` with a `main` method.
2. Declare and initialize an integer array named `scores` containing these exact values: `{85, 92, 78, 90, 88}`.
3. Declare an integer variable named `sum` and initialize it to `0`.
4. Use a standard `for` loop to traverse the array from index `0` to `scores.length - 1` and add each score to the `sum` variable.
5. After the loop, calculate the average score. Make sure to cast the `sum` to `double` so that you don't lose the decimal fraction! Store this in a variable named `average`.
6. Print the sum and average exactly as shown in the expected output below.

### Expected Output:
```
Total Sum: 433
Average Score: 86.6
```

### Code Skeleton
```java
public class ArrayAverage {
    public static void main(String[] args) {
        // Declare and initialize the array
        int[] scores = {85, 92, 78, 90, 88};

        int sum = 0;

        // TODO 1: Use a for loop to add all scores together into 'sum'
        for (int i = 0; i < scores.length; i++) {
            // Your code here
        }

        // TODO 2: Calculate the average (don't forget to cast!)
        double average = 0.0; // Update this formula

        // Print results
        System.out.println("Total Sum: " + sum);
        System.out.println("Average Score: " + average);
    }
}
```
