---
title: Lab 5
nav_order: 2
parent: 2026 DSA
layout: page
math: mathjax


---
# Lab 5 Iteration and Loops (while & for)
{: .no_toc}

## Goals:
{: .no_toc}
* Understand the structure and mechanics of loops in Java.
* Learn when to use an indefinite `while` loop versus a definite `for` loop.
* Implement algorithms for cumulative calculation (sums, products, counts) using loops.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Example A: Cumulative Sum with a `while` Loop

Easy
{: .label .label-green }

When solving programming problems, you often need to repeat a block of code until a certain condition is met. This is called **iteration**.

A `while` loop repeatedly executes a block of code as long as a boolean condition remains `true`. A common loop pattern is the **cumulative sum**, where you maintain a running total in a variable and add values to it on each iteration.

Here is a program `WhileSum` that takes an integer $$N$$ as a command-line argument and calculates the sum of all integers from 1 to $$N$$ using a `while` loop.

```java
// in file WhileSum.java
public class WhileSum {
    
    public static void main(String[] args) {
        // Read N from the command line
        int N = Integer.parseInt(args[0]);
        
        int sum = 0;   // Accumulator to store the running sum
        int i = 1;     // Loop counter/control variable
        
        while (i <= N) {
            sum += i;  // Add the current value of i to sum
            i++;       // Increment the counter to eventually end the loop
        }
        
        System.out.println("The sum from 1 to " + N + " using a while loop is: " + sum);
    }
}
```

---

## Task A: Cumulative Product (Factorial Calculator)

Easy
{: .label .label-green }

Using the `WhileSum` example as a template, write a program `Factorial` that calculates the factorial of a non-negative integer $$N$$ (denoted as $$N!$$).

The factorial of $$N$$ is the product of all positive integers less than or equal to $$N$$. For example:
$$5! = 1 \times 2 \times 3 \times 4 \times 5 = 120$$

#### Rules for Mimicking:
1. Refer directly to `WhileSum.java`. Instead of initializing an accumulator to `0` and **adding** numbers, you should initialize a product accumulator to `1` and **multiply** numbers on each iteration.
2. Implement your solution using a `while` loop.

#### Sample Input
```bash
java Factorial 5
java Factorial 1
```

#### Sample Output
```
5! = 120
1! = 1
```

---

## Example B: Cumulative Sum with a `for` Loop

Easy
{: .label .label-green }

A `for` loop is a compact way to write loops that are controlled by a counter. It combines initialization, condition evaluation, and increment/update in a single line, making it excellent for definite (count-controlled) iteration.

Here is a program `ForSum` that accomplishes the exact same sum calculation as Example A, but uses a compact `for` loop.

```java
// in file ForSum.java
public class ForSum {
    
    public static void main(String[] args) {
        // Read N from the command line
        int N = Integer.parseInt(args[0]);
        
        int sum = 0;   // Accumulator to store the running sum
        
        // for (initialization; condition; update)
        for (int i = 1; i <= N; i++) {
            sum += i;  // Add the current value of i to sum
        }
        
        System.out.println("The sum from 1 to " + N + " using a for loop is: " + sum);
    }
}
```

---

## Task B: Sum of Odd Numbers

Moderate
{: .label .label-purple }

Using the `ForSum` example as a template, write a program `SumOfOdds` that takes an integer $$N$$ as a command-line argument and calculates the sum of all **odd** integers from 1 to $$N$$.

For example, if $$N = 6$$, the odd integers from 1 to 6 are 1, 3, and 5. The sum is:
$$1 + 3 + 5 = 9$$

#### Rules & Hints for Mimicking:
1. Refer directly to `ForSum.java`.
2. Inside your loop, you need to decide whether to add $$i$$ to your sum or not. You can achieve this in two different ways (choose one):
   * **Approach 1 (Conditional check)**: Keep the loop update as `i++` and use an `if` statement with the modulo operator (`%`) inside the loop to only accumulate $$i$$ when it is odd (i.e., `i % 2 != 0`).
   * **Approach 2 (Custom step)**: Set up your `for` loop update statement to increment $$i$$ by 2 on each iteration (i.e., `i += 2`) to automatically skip even numbers.

#### Sample Input
```bash
java SumOfOdds 6
java SumOfOdds 10
```

#### Sample Output
```
The sum of odds from 1 to 6 is: 9
The sum of odds from 1 to 10 is: 25
```
