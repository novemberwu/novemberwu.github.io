---
title: Lab 5
nav_order: 5
parent: Labs
layout: page
math: mathjax

---
# Lab 5 Compound Assignment and Documentation
{: .no_toc}

## Goals
{: .no_toc}
* Able to process data types with desired precision requirements.
* Able to use compound assignments and the `Math` library.
* Able to write clear, professional comments (including Javadoc style) to make programs highly readable.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Example A: Annual Compound Interest

Easy
{: .label .label-green }

When writing programs that compute financial formulas, you often use the `Math` library (like `Math.pow`) to handle exponents. It is also standard engineering practice to document your code using Javadoc comments (`/** ... */`) and inline comments (`//`) to explain variable definitions and formula calculations.

Here is a program `AnnualInterest` that calculates compound interest compounded annually:
$$A = P(1 + r)^t$$

```java
/**
 * This program calculates compound interest compounded annually.
 * Formula: A = P * (1 + r)^t
 * 
 * @author Rachel Wu
 */
public class AnnualInterest {
    
    public static void main(String[] args) {
        double principal = 1000.0; // Initial investment in dollars
        double rate = 0.05;        // Annual interest rate (5%)
        int years = 10;            // Investment duration in years
        
        // Calculate the final accumulated amount using Math.pow
        double amount = principal * Math.pow(1 + rate, years);
        
        System.out.println("Initial investment: $" + principal);
        System.out.println("Annual interest rate: " + (rate * 100) + "%");
        System.out.println("Amount after " + years + " years: $" + amount);
    }
}
```

---

## Task A: Continuous Compound Interest

Moderate
{: .label .label-purple }

Using the `AnnualInterest` example as a template, write a program `ContinuousInterest` that calculates and prints the amount of money you would have after $$t$$ years if you invested $$P$$ dollars at an annual interest rate $$r$$ (compounded continuously).

{: .note }
> The final value is given by the formula:
> $$E = P \cdot e^{r \cdot t}$$
> 
> *Hint:* Use `Math.exp(x)` to compute $$e^x$$.

### Requirements:
1. Parse three command-line arguments: $$P$$ (double), $$r$$ (double, as a decimal e.g. `0.05` for 5%), and $$t$$ (double).
2. Include Javadoc comments at the top of your class and clear inline comments explaining your calculations.

### Sample Input
```bash
java ContinuousInterest 1000.0 0.05 10
```

### Sample Output
```
Initial investment: $1000.0
Continuously compounded amount after 10.0 years: $1648.7212707001282
```

---

## Example B: Fair Item Distribution with Random Residual

Easy
{: .label .label-green }

When dividing a set of resources among a group of people, you can find the base division share using integer division `/`, and the leftover remainder using the modulo operator `%`. You can then use `Math.random()` to randomly pick who gets the leftover items.

Here is a program `DistributeItems` that demonstrates this concept:

```java
/**
 * This program demonstrates how to distribute items evenly among a group.
 * Any leftover items (the residual) are given to a randomly selected person.
 */
public class DistributeItems {
    
    public static void main(String[] args) {
        int totalItems = 17;
        int numPeople = 5;
        
        // Calculate the base share for each person
        int baseShare = totalItems / numPeople;
        
        // Calculate the leftover items (remainder / residual)
        int remainder = totalItems % numPeople;
        
        // Randomly select one person (index from 0 to numPeople - 1) to get the remainder
        // Math.random() returns a double from [0.0, 1.0)
        int luckyPerson = (int) (Math.random() * numPeople);
        
        System.out.println("Distributing " + totalItems + " items among " + numPeople + " people:");
        System.out.println("Lucky person getting the residual: Person " + (luckyPerson + 1));
        System.out.println();
        
        // Loop and print the distributed amount for each person
        for (int i = 0; i < numPeople; i++) {
            int share = baseShare;
            if (i == luckyPerson) {
                share += remainder; // Lucky person takes the residual
            }
            System.out.println("Person " + (i + 1) + " receives: " + share);
        }
    }
}
```

---

## Task B: Distribute Money

Moderate
{: .label .label-purple }

Using the `DistributeItems` example as a template, write a program `DistributeMoney` that distributes a total dollar amount evenly among a specified number of people, giving the remainder (if any) to a randomly selected person.

### Requirements:
1. The program must accept exactly two command-line arguments:
   * First argument: the total dollar amount (double, e.g., `2.5` representing $2.50).
   * Second argument: the number of people (int, $$\le 10$$).
2. Print the distributed amount for each person on a single line, separated by spaces.
3. If the total amount cannot be evenly divided, a **random person** receives the residual amount.

{: .note }
> *Precision Hint:* To avoid double division rounding issues and find the exact residual down to the decimal units (e.g. 0.1), you can scale your values. For example, convert the dollar amount to dimes (multiply by 10) or cents (multiply by 100), perform integer operations, and scale them back when printing.
> 
> Adding up each person's printed portion must equal the exact total amount of money started with.

### Sample Input
```bash
java DistributeMoney 2.5 3
```

### Sample Output (The random person could vary)
```
0.8 0.8 0.9
```
*(Explanation: $$0.8 \times 3 = 2.4$$, leaving $$0.1$$ residual which is given to a random person to make $$0.9$$. Total sum is $$0.8 + 0.8 + 0.9 = 2.5$$).*
