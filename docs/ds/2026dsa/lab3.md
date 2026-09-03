---
title: Lab 3
nav_order: 2
parent: 2026 DSA
layout: page
math: mathjax


---
# Lab 2 String

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Example A: String demo

Easy
{: .label .label-green }

String and operator
```java

public class StringDemo {
    public static void main(String[] args){

        String s = "This is a String";
        System.out.println(s);

        String t = new String("This is a another String");
        System.out.println(t);

        s = s + "Hello";
        System.out.println(s);

        s = s + 9; //implicit cast from integer 9 to String "9"


        s = "99";
        int a = Integer.valueOf(s);
        System.out.println(s);
    }
}

```


## Example B: String equality
To compare the equality of the two string objects. use keyword ```java equals```, not ```java ==```

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
