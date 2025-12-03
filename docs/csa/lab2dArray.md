---
title: G10 Lab 2D Array
nav_order: 14
parent: Labs
layout: page

---
# Lab 2D Array
{: .no_toc }
## Goals:
{: .no_toc }
* Able to use create and initialize 2D array
* Able to implement 2D Array Algorithms

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}


## Task A: 2D Array Declaration and Initialization

Moderate
{: .label .label-purple }

* create 2D array the type is ```String```
* each row present a student and his/her selective courses
* the first element of each row is student's name. The rests are the courses

Sample code:
```java
String[][] courses ={
        {"Rachel","Computer Science A","Computer Science Principle"}
        // adding more students here
};

```

## Task B: Write a method to get the number of course of a given student
* Iterate each row (presenting each student) of the array
* Get the first element of each row
* Test if first element equals to the given student name
* Count the course number if true

Sample code
```java
public class StudentCourses{
    public static void main(String[] args){
        String student = "Rachel";
        int courseNum = getCourseNumber(courses, student);
        System.out.println(courseNum);
    }
    
    private static int getCourseNumber(String[][] courses, String studentName){
        // Your implementation here    
    }
}


```
Sample output
```java
2
```
