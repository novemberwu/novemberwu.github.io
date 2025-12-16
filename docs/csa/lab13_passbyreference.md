---
title: Lab Object Reference as Parameter
nav_order: 20
parent: Labs
layout: page

---
# Lab Object Reference as Parameter
{: .no_toc }
## Goals:
{: .no_toc }
* Understand how the object reference is passed to methods


## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Task A Investigate and Modify

```java
public class Course {

    private String name;
    private boolean status;

    public Course(String name, boolean status) {
        this.name = name;
        this.status = status;
    }

    public void setStatus(boolean newStatus) {
        status = newStatus;
    }

    public String toString() {
        String text = name + ": ";

        if (status) {
            text = text + "Enrolled";
        }
        else {
            text = text + "Dropped";
        }

        return text;
    }

}


public class Student {

    private String name;
    private Course newCourse;

    public Student(String name, Course newCourse) {
        this.name = name;
        this.newCourse = newCourse;
    }

    public void dropCourse(Course theCourse) {
        theCourse.setStatus(false);
    }

    public String toString() {
        return name + "\n" + newCourse;
    }

}

public class StudentRunner {
    public static void main(String[] args) {

        boolean isEnrolled = true;

        Course csCourse = new Course("Computer Science", isEnrolled);
        Student betsy = new Student("Betsy", csCourse);

        System.out.println(betsy);

        /* ---- 🔎 ADD YOUR CODE BELOW THIS LINE ---- */


    }
}


```




Experiment with the program by making the following modifications, then run the program to observe the results.

✅ TO DO #1: Look at the ```setStatus()``` method in the Course class. What do you notice about its parameter? Does the parameter get a copy of the boolean value when the method is called?

✅ TO DO #2: In the StudentRunner class, add the following code:
```System.out.println("isEnrolled = " + isEnrolled);```
Run the program. What do you notice about the output? Why do you think this happened?




## Task B Pass Reference to Method
✅ TO DO: What do you think will happen if you add the following line?
In the StudentRunner class, add the following code:
```java
betsy.dropCourse(csCourse);
System.out.println(csCourse);

```
Run the program. What do you notice about the output? Why do you think this happened?



# Check for understanding
```java
public class QuizScore {
  private String studentId;
  private double score;

  public QuizScore(String stuId, double stuScore) {
    studentId = stuId;
    score = stuScore;
  }

  public double getScore() {
    return score;
  }

  public void bonus(int b) {
    score += score * b / 100.0;
  }
}

```

Assume that the following two methods appear in a class other than QuizScore.
```java
public void mystery(QuizScore s, int b) {
  s.bonus(b);
  b = 0;
}

public String start() {
  QuizScore mw = new QuizScore("17743", 80.0);
  int bonus = 5;
  mystery(mw, bonus);
  return mw.getScore() + " " + bonus;
}
```
What is returned as a result of the call start()?

A.  84.0 0
{: .btn .btn-outline }

B.  84.0 5
{: .btn .btn-outline }

C.  80.0 0
{: .btn .btn-outline }

D.  80.0 5
{: .btn .btn-outline }

E.  17743 80.0
{: .btn .btn-outline }



