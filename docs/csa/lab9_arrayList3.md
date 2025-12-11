---
title: Lab ArrayList Modification
nav_order: 16
parent: Labs
layout: page

---
# Lab Array List Modification
{: .no_toc }
## Goals:
{: .no_toc }
* Able to avoid ConcurrentModificationException while modifying ArrayList


## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}


## Task A: What do you think the program do 

Easy
{: .label .label-green }
Take a look at the code in this program. Write down what you think this program will do. There are no wrong answers!

Sample code:
```java

import java.util.ArrayList;

public class ListRunner {
    public static void main(String[] args) {

        ArrayList<Integer> numList = new ArrayList<Integer>();
        numList.add(42);
        numList.add(66);
        numList.add(71);
        numList.add(12);
        numList.add(39);

        System.out.println("Original list: " + numList + "\nSize: " + numList.size());

        for (int value : numList) {
            int randomNum = (int)(Math.random() * 50) + 10;
            numList.add(randomNum);
        }

        System.out.println("New list: " + numList + "\nSize: " + numList.size());

        for (int index = 0; index < numList.size(); index++) {
            System.out.println(numList.remove(2) + " removed");
            System.out.println("\nNew list: " + numList + "\nSize: " + numList.size());
        }
    }
}


```

## Task B: What error is shown in the console? Why do you think this happened?
Moderate
{: .label .label-purple }
Change the foreach loop to a regular for loop then run the program to observe the results. Does the error still occur? What happens instead?

## Task C: Modify remove
Moderate
{: .label .label-purple }

Experiment with the program by making the following modifications, then run the program to observe the results.

* TO DO #1: What does the remove() method remove? Try changing the 2 to a different value, like 1. Run the program to observe the results after each change.

* TO DO #2: What happens if you change the argument for the remove() method to the loop control variable? Try changing the argument to index and run the program to observe the results

## Task D: Modify remove until ArrayList is cleared
Moderate
{: .label .label-purple }

* TO DO: After the remove() inside the for loop, add the line index--; then run the program.

How does this change the result? Why do you think this is needed?