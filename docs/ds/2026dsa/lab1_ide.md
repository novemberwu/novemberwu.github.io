---
title: Lab 1
layout: page
nav_order: 2
parent: 2026 DSA

---

# Lab 1: Development Environment Setup & Hello World
{: .no_toc }
---
This guide walks you through configuring your Java environment, writing, compiling, and running your first Java program

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}


### 1. Install Java Development Kit

Complete each step below to install the necessary tools on your operating system (macOS, Windows, or Linux).


Use **JDK 17 or above** to compile and run course assignments.

* **Guide**: 
  
    Follow [Princeton IntroCS Java Setup Guide](https://introcs.cs.princeton.edu/java/code/) to install development environment
  * **MacOS** [MacOS Guide/ part 0](https://lift.cs.princeton.edu/java/mac/) 
  * **Windows** [Windows Guide/ part 0](https://lift.cs.princeton.edu/java/windows/)
  * **Linux** [Linux Guide/ part 0](https://lift.cs.princeton.edu/java/linux/)
  
* **Installation**:
1. Download the installer package for your operating system (`.msi` for Windows, `.pkg` for macOS, or package manager for Linux).
2. Complete installation.


---

### 2. Verify Your Environment

Open a new terminal window or command prompt and run the following verification checks:

```bash
javac -version
java -version
```

Both `javac` and `java` must output a version matching `21.x.x`.

---
## Task A: Hello World
Easy
{: .label .label-green }
1. Create a new project in your IDE
2. Create a new java file with name ```HelloWorld.java```
3. Paste the following code into the file

```java
public class HelloWorld{

  public static void main(String[] args){
     System.out.println("Hello, World");
  }
}
```



Compile and run the **HelloWorld** program in command line

```
javac HelloWorld.java
java HelloWorld
```
Expected output is

```
Hello, World

Process finished with exit code 0
```

{: .note }
You may need to specify your java class path in order to compile the code


## Example B: Using Argument

Easy
{: .label .label-green }

You can pass inputs to your program from the command line using **command-line arguments**. In Java, these arguments are captured in the `String[] args` array in the `main` method. The first argument is stored in `args[0]`.

Here is the code for `UseArgument.java`:

```java
public class UseArgument {
    public static void main(String[] args) {
        System.out.print("Hi, ");
        System.out.print(args[0]);
        System.out.println(". How are you?");
    }
}
```

### Sample Input
```bash
java UseArgument Rachel
```

### Sample Output
``` 
Hi, Rachel. How are you?

Process finished with exit code 0
```

{: .note }
You may need to install the IntelliJ LIFT plugin to run arguments efficiently inside your IDE.

---

## Task B: Reverse Three Arguments

Easy
{: .label .label-green }

Write a program `ThreeArguments` that takes **three** command-line arguments and prints a greeting with the arguments in **reverse order**. 

If you pass three names, say `Alice`, `Bob`, and `Carol`, the program should print a friendly message with the names listed as `Carol`, `Bob`, and `Alice`.

{: .note }
Since Java arrays are zero-indexed, the three arguments will be stored in `args[0]`, `args[1]`, and `args[2]`.

### Sample Input
```bash
java ThreeArguments Alice Bob Carol
```

### Sample Output
```
Hi, Carol, Bob, and Alice.
```

