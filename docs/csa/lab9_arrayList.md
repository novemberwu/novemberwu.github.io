---
title: Lab ArrayList Basic
nav_order: 14
parent: Labs
layout: page

---
# Lab Array List
{: .no_toc }
## Goals:
{: .no_toc }
* Able to use create and initialize ArrayList 
* Able to use Array methods



* ![](../../assets/images/ArrayListCheat.png)

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Task A: Explore ArrayList Add and Size

Easy
{: .label .label-green }

* **TODO #1**: What is the size of ```numbers```
* **TODO #2**: Adding ```System.out.println(numbers.size());``` into the main block then run program
```java
public class ListRunner {
      public static void main(String[] args) {
    
        ArrayList<Integer> numbers = new ArrayList<Integer>();
    
        numbers.add(10);
        numbers.add(20);
        numbers.add(0, 30);
        
      }
}
```

*  **TODO #3**: What is the difference between the add() method with one parameter and the add() method with two parameters?
*  **TODO #4**: After the last add() method, add the line System.out.println(numbers); then run the program
What is printed? What can you conclude about both version of add()

## Task B: Explore ArrayList Get/Set
Easy
{: .label .label-green }
* **TODO #1** What is the first number of ```numbers```
* **TODO #2** Adding ```System.out.println(numbers.get(0));``` into the main block then run program
* **TODO #3** Adding ```System.out.println(numbers.get(3));``` into the main block then run program, what will happen?
* **TODO #4** Adding ```numbers.set(0, 999);``` into the main block then run program. then print the arraylist

## Task C: Explore ArrayList indexOf/ Contains

Easy
{: .label .label-green }
* **TODO #1** What is the first number of ```numbers```
* **TODO #2** Adding ```System.out.println(numbers.indexOf(20));``` into the main block then run program
* **TODO #3** Adding ```System.out.println(numbers.contains(3));``` into the main block then run program, what will happen?


