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

        String[][] courses ={
                {"Rachel","Computer Science A","Computer Science Principle"},
                // adding more students here
                {"Ruiqi", "AP Physics", "CSA", "Calculus"},
                {"Xiaobing", "CSA","Pre-Chemistry" }
        };

        String student = "Ruiqi";
        int courseNum = getCourseNumber(courses, student);
        System.out.println(courseNum);

    }


    private static int getCourseNumber(String[][] courses, String studentName){
        // Your implementation here
        for(int i = 0 ; i < courses.length;i++){
            String[] stu = courses[i];
            if(stu[0].equals(studentName)){
                return stu.length -1;
            }
        }
        return -1;
    }
}


```
Sample output
```
2
```

## Task C: Calculate average grade of each class
You are given a 2D array, each row stores the grades of all students in one class. 
i.e. the first row stores all grades of students from class one.
The second row stores grades of students from class two

You write a program to calculate the average grade of each class, and print out the class number with 
the highest average grade, and its average grade

Sample code
```java
public class GradeCalc{
    public static void gradeCalc(int[][] grades){
        // print the class number, print the average grade of the class
        // print the class number, print the average grade of the class
        double maxAverage = -1;
        int maxClass = -1;
        for (int i = 0; i < grades.length; i++) {
            double classSum = 0;
            for (int j = 0; j < grades[i].length; j++) {
                classSum += grades[i][j];
            }
            classSum /= grades[i].length;
            System.out.printf("\nClass: %d  average grade %.2f", i, classSum);
            if( classSum > maxAverage){
                maxClass = i;
                maxAverage = classSum;
            }

        }

        System.out.printf("\n\n **** Class %d has highest average grade %.2f", maxClass, maxAverage);
    }
    public static void main(String[] args){
        int[][] grades = new int[4][];
        grades[0] = new int[]{99, 98, 40, 88, 79, 90}; // grades from class one
        grades[1] = new int[]{66, 68, 77, 99, 100, 83}; // grades from class two
        grades[2] = new int[]{36, 68, 77, 99, 100, 83};
        grades[3] = new int[]{96, 98, 77, 99, 10, 22, 66};

        gradeCalc(grades);
    }
}

```

Sample output
```text
Class: 0  average grade 82.33
Class: 1  average grade 82.17
Class: 2  average grade 77.17
Class: 3  average grade 66.86

 **** Class 0 has highest average grade 82.33
```