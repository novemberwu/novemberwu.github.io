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

