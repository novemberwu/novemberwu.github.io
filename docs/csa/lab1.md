---
title: Lab 1
nav_order: 1
parent: Labs
layout: page

---

# (AP) Lab Computer Science A
{: .no_toc }

## Goal
* Set up the development environment
* Able to compile the java program
* Able to run the java program with arguments

## Get Started

### IDE and Java SDK setup
* [Guide](https://introcs.cs.princeton.edu/java/code/)

{: .note }
JDK 11 is needed to compile and run the code.

### Task A: Hello World 
Easy
{: .label .label-green }

* Run Java program in IDE
Compile and run the **11hello/HelloWorld** program in Intellij. 
Expected output is

```
Hello, World

Process finished with exit code 0
```
* Run Java program in Command line
Compile and run the 11hello/HelloWorld program in command line

```
cd 11hello
javac HelloWorld.java
java HelloWorld
```
this should produce the same output as previous one

{: .note } 
You may need to specify your java class path in order to compile the code


### Task B: Using Argument 
Easy
{: .label .label-green }
Compile and run the program **11hello/UseArgument** in IDE with populated input argument

Sample input
```
java UseArgument Rachel
```
Sample output
``` 
Hi, Rachel. How are you?

Process finished with exit code 0
```

{: .note }
You may need to install intellij LIFT plugin to run argument efficiently


### Task C: Modify UseArgument 

Easy
{: .label .label-green }

Modify UseArgument.java to make a program UseThree.java that takes three names and prints out a proper sentence with the names in the reverse of the order given, so that for example, "java UseThree Alice Bob Carol" gives "Hi Carol, Bob, and Alice.".

Sample input
```
java UseThree Alice Bob Carol
```
Sample output

```
Hi Carol, Bob and Alice

```


