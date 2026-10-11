---
title: Lab 12 Stacks & Queues APIs and Applications
nav_order: 8
parent: 2026 DSA
layout: page
math: mathjax
---

# Lab 12: Stacks & Queues APIs and Applications
{: .no_toc}

## Goals:
{: .no_toc}
* Understand the core abstractions of **Stacks** and **Queues** as fundamental collection Abstract Data Types (ADTs).
* Distinguish between **LIFO (Last-In, First-Out)** and **FIFO (First-In, First-Out)** access patterns and understand where each naturally arises in computing.
* Master the standard APIs:
  * **Stack:** `push(item)`, `pop()`, `isEmpty()`, and `size()`.
  * **Queue:** `enqueue(item)`, `dequeue()`, `isEmpty()`, and `size()`.
* Trace step-by-step state changes and return values for both data structures given a sequence of operations.
* Apply a **Queue** as an elastic buffer to ingest an arbitrary stream of input tokens into fixed-size arrays.
* Understand **Infix** versus **Postfix (Reverse Polish Notation / RPN)** arithmetic expressions and appreciate why postfix notation eliminates the need for parentheses.
* Build a stack-based **Postfix Arithmetic Evaluator** capable of computing multi-operator mathematical expressions.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Stacks & Queues APIs (Slides 1–3)

In previous labs, we explored lists (`SLList`, `DLList`, `AList`) that allowed inserting, removing, and inspecting elements at arbitrary indices. However, many foundational computing problems do not require arbitrary access. Instead, they require strict, predictable rules about **which item is removed next**.

Stacks and Queues are the two primary ADTs that govern sequential data flow (Slide 3):

```
       STACK (LIFO)                               QUEUE (FIFO)
  Add to top    Take from top               Take from front (dequeue)
       │            ▲                                   ▲
       ▼            │                                   │
   +--------------------+                       +--------------------+
   |   Most Recent      |                       |    Least Recent    |
   +--------------------+                       +--------------------+
   |   ...              |                       |    ...             |
   +--------------------+                       +--------------------+
   |   Oldest Item      |                       |    Most Recent     |
   +--------------------+                       +--------------------+
   (Last-In, First-Out)                                 ▲
                                                        │
                                                Add to back (enqueue)
                                                (First-In, First-Out)
```

### 1. The Stack ADT (Last-In, First-Out / LIFO)
A **Stack** models a vertical pile of items (like a stack of cafeteria trays or books on a desk). You can only add items to the top, and you can only take items from the top.
* **`push(Type item)`**: Adds an item to the collection (onto the top).
* **`pop()`**: Removes and returns the item **most recently added** (from the top).
* **`isEmpty()`**: Returns `true` if the collection contains no items, `false` otherwise.
* **`size()`**: Returns the number of items currently in the collection.

### 2. The Queue ADT (First-In, First-Out / FIFO)
A **Queue** models a line of people waiting for service (like cars at a toll booth or print jobs sent to a printer). Items enter at the back (rear) and exit from the front.
* **`enqueue(Type item)`**: Adds an item to the collection (at the end / rear).
* **`dequeue()`**: Removes and returns the item **least recently added** (from the front).
* **`isEmpty()`**: Returns `true` if the collection contains no items, `false` otherwise.
* **`size()`**: Returns the number of items currently in the collection.

> **Key Characteristic (Slide 3):** Stacks and queues both arise naturally in countless applications. A crucial design property is that there is **no predefined limit** on the size of the collection.

---

## Part 2: Operation Tracing (Slides 4–5)

To build a reliable mental model of Stacks and Queues, let's trace how each data structure handles the exact same stream of input tokens (Slide 4, 5).

Suppose we process the following sequence of words, where each word represents an insertion, and each hyphen (`-`) represents a deletion:

$$$$\text{to} \quad \text{be} \quad \text{or} \quad \text{not} \quad \text{to} \quad - \quad \text{be} \quad - \quad - \quad \text{that} \quad - \quad - \quad - \quad \text{is}$$$$

### Example A1: Stack Trace (LIFO) (Slide 4)
In a stack, words are **pushed** to the top, and hyphens (`-`) **pop** the most recently added item from the top:

| Token | Operation | Stack Contents (Bottom $$\rightarrow$$ Top) | Returned by `pop()` |
| :---: | :---: | :--- | :---: |
| `to` | `push("to")` | `[to]` | — |
| `be` | `push("be")` | `[to, be]` | — |
| `or` | `push("or")` | `[to, be, or]` | — |
| `not` | `push("not")` | `[to, be, or, not]` | — |
| `to` | `push("to")` | `[to, be, or, not, to]` | — |
| `-` | `pop()` | `[to, be, or, not]` | **`to`** |
| `be` | `push("be")` | `[to, be, or, not, be]` | — |
| `-` | `pop()` | `[to, be, or, not]` | **`be`** |
| `-` | `pop()` | `[to, be, or]` | **`not`** |
| `that`| `push("that")` | `[to, be, or, that]` | — |
| `-` | `pop()` | `[to, be, or]` | **`that`** |
| `-` | `pop()` | `[to, be]` | **`or`** |
| `-` | `pop()` | `[to]` | **`be`** |
| `is` | `push("is")` | `[to, is]` | — |

* **Sequence of words returned by `pop()`:** `to be not that or be`
* **Final Stack Contents:** `[to, is]` (size = 2)

---

### Example A2: Queue Trace (FIFO) (Slide 5)
In a queue, words are **enqueued** at the end, and hyphens (`-`) **dequeue** the least recently added item from the front:

| Token | Operation | Queue Contents (Front $$\rightarrow$$ Rear) | Returned by `dequeue()` |
| :---: | :---: | :--- | :---: |
| `to` | `enqueue("to")` | `[to]` | — |
| `be` | `enqueue("be")` | `[to, be]` | — |
| `or` | `enqueue("or")` | `[to, be, or]` | — |
| `not` | `enqueue("not")`| `[to, be, or, not]` | — |
| `to` | `enqueue("to")` | `[to, be, or, not, to]` | — |
| `-` | `dequeue()` | `[be, or, not, to]` | **`to`** |
| `be` | `enqueue("be")` | `[be, or, not, to, be]` | — |
| `-` | `dequeue()` | `[or, not, to, be]` | **`be`** |
| `-` | `dequeue()` | `[not, to, be]` | **`or`** |
| `that`| `enqueue("that")`| `[not, to, be, that]` | — |
| `-` | `dequeue()` | `[to, be, that]` | **`not`** |
| `-` | `dequeue()` | `[be, that]` | **`to`** |
| `-` | `dequeue()` | `[that]` | **`be`** |
| `is` | `enqueue("is")` | `[that, is]` | — |

* **Sequence of words returned by `dequeue()`:** `to be or not to be`
* **Final Queue Contents:** `[that, is]` (size = 2)

---

## Task A: Stack & Queue Tracing Detective

Easy
{: .label .label-green }

Practice tracing both data structures by hand on the following sequence of tokens, where letters are inserted and `-` indicates removal:

$$$$\text{E} \quad \text{A} \quad \text{S} \quad * \quad \text{Y} \quad * \quad \text{Q} \quad \text{U} \quad \text{E} \quad * \quad * \quad * \quad \text{S} \quad \text{T} \quad * \quad * \quad * \quad \text{I} \quad \text{O} \quad \text{N} \quad * \quad * \quad *$$$$

*(Here, each letter is an insert, and each asterisk `*` represents a removal).*

### Questions:
1. **Stack Execution:**
   * In what order are letters removed from the collection by `pop()`?
   * What letters remain on the stack after all tokens are processed, and in what order (bottom to top)?
2. **Queue Execution:**
   * In what order are letters removed from the collection by `dequeue()`?
   * What letters remain in the queue after all tokens are processed, and in what order (front to rear)?

<details markdown="1">
<summary><b>Click to check your trace solutions:</b></summary>

**1. Stack (LIFO):**
* Letters popped: `S Y E U Q T S A N O I`
* Final stack contents (bottom to top): `[E]`

**2. Queue (FIFO):**
* Letters dequeued: `E A S Y Q U E S T I O`
* Final queue contents (front to rear): `[N]`

</details>

---

## Part 3: Applications & The Queue Client (Slides 6–8)

Why do we need Stacks and Queues when general lists already exist (Slide 6, 7)?

### Real-World & Computational Applications (Slide 7):
* **Queues (FIFO):**
  * **First-Come, First-Served (FCFS) Resource Allocation:** Managing traffic intersections, shared printers, customer support lines.
  * **Asynchronous Data Transfer:** Input/output stream buffering (e.g., keyboard keystroke buffers, network socket packets).
  * **Discrete-Event Simulation:** Modeling real-world lines, customer service flows, supermarket checkouts.
* **Stacks (LIFO):**
  * **Call Stack:** Managing method invocations and local variable frames in programming languages (including recursion).
  * **Expression Parsing & Evaluation:** Compilers and interpreters evaluating arithmetic expressions.
  * **Undo/Redo Mechanisms:** Word processors and IDEs tracking user operations (Ctrl+Z).
  * **Backtracking Algorithms:** Navigating mazes, tree/graph depth-first search.

---

## Example B: Queue Client — Ingesting Dynamic Input (Slide 8)

A classic challenge in programming is reading an unknown number of items from standard input into a fixed-size Java array. Because arrays require a fixed size when created (`new String[N]`), you cannot directly allocate the array before knowing how many items the user will provide.

A **Queue** solves this problem cleanly (Slide 8):
1. Read strings one by one from standard input and **enqueue** each string.
2. Once the input stream terminates, query `q.size()` to get the exact count ($$N$$).
3. Allocate an array of size $$N$$: `new String[N]`.
4. **Dequeue** each string directly into the array in its original FIFO order!

Here is the implementation of `QEx.java` (Slide 8):

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class QEx {
    public static String[] readAllStrings(Scanner sc) {
        // Step 1: Use a Queue to dynamically collect strings
        Queue<String> q = new LinkedList<>();
        while (sc.hasNext()) {
            q.add(sc.next()); // enqueue
        }

        // Step 2 & 3: Allocate array matching the exact queue size
        int n = q.size();
        String[] words = new String[n];

        // Step 4: Dequeue strings into the array in original FIFO order
        for (int i = 0; i < n; i++) {
            words[i] = q.remove(); // dequeue
        }

        return words;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner("apple banana cherry date elderberry");
        String[] words = readAllStrings(sc);
        sc.close();

        System.out.println("Array size allocated: " + words.length);
        for (int i = 0; i < words.length; i++) {
            System.out.println("words[" + i + "] = " + words[i]);
        }
    }
}
```

---

## Task B: Transaction Batch Processor

Medium
{: .label .label-yellow }

In banking systems, transactions arrive continuously over network streams. Before writing them to disk, batches of transactions of unknown size must be buffered in order and packaged into arrays for bulk processing.

Following the pattern from Slide 8 (`QEx`), you will implement `TransactionBuffer.java`:
1. Use a `Queue<String>` to collect an unknown number of incoming transaction IDs from an input stream.
2. Once the stream ends, determine the exact batch size using `q.size()`.
3. Allocate a `String[]` array of that exact size.
4. Dequeue each transaction into the array in FIFO arrival order.

### Instructions:
1. Complete the method `public static String[] bufferTransactions(String stream)`:
   * Read tokens from the `Scanner` while `sc.hasNext()`.
   * Enqueue each transaction ID into the queue.
   * Query the queue's size $N$.
   * Allocate an array of length $N$.
   * Dequeue each transaction from the queue into the array at consecutive indices.
   * Return the populated array.
2. In `main`, test your method with the sample input and verify the printed output.

### Code Skeleton for `TransactionBuffer.java`:
```java
import java.util.ArrayDeque;
import java.util.Queue;
import java.util.Scanner;

public class TransactionBuffer {

    /**
     * Reads transactions from an input string, enqueuing incoming records
     * and returning them packed in a fixed-size array in FIFO order.
     */
    public static String[] bufferTransactions(String stream) {
        Scanner sc = new Scanner(stream);
        Queue<String> q = new ArrayDeque<>();

        // TODO 1: Read all tokens while sc.hasNext() and enqueue them into q
        

        // TODO 2: Determine the total number of buffered transactions using q.size()
        

        // TODO 3: Allocate a String[] array of the exact size needed
        

        // TODO 4: Dequeue each transaction into the array in FIFO order
        

        sc.close();
        // TODO 5: Return the populated array
        return null;
    }

    public static void main(String[] args) {
        String input = "TX-01 TX-02 TX-03 TX-04 TX-05";

        // TODO 6: Call bufferTransactions and print each transaction and total count
        
    }
}
```

### Expected Output:
```
Buffered 5 transactions:
[0] TX-01
[1] TX-02
[2] TX-03
[3] TX-04
[4] TX-05
```

---

## Part 4: Stack Applications & Postfix Evaluation (Slides 9–11)

### 1. Infix vs. Postfix Expressions (Slide 9)
In standard mathematical notation, operators are written **between** operands. This is known as **Infix Notation**:

$$(1 + ((2 + 3) \times (4 \times 5))) = (1 + (5 \times 20)) = 101$$

Infix notation requires complex precedence rules (multiplication before addition) and parentheses to override precedence.

In the 1920s, Polish logician **Jan Łukasiewicz** (1878–1956) showed that if you write operators **after** their operands, known as **Postfix Notation** or **Reverse Polish Notation (RPN)**:

$$\text{Postfix:} \quad 1 \quad 2 \quad 3 \quad + \quad 4 \quad 5 \quad \times \quad \times \quad +$$

**Remarkable Property (Slide 9):** **No parentheses are needed!** There is only one mathematically unambiguous way to evaluate a postfix expression:
* $$1 \quad 2 \quad 3 \quad + \quad 4 \quad 5 \quad * \quad * \quad +$$
* $$1 \quad (2 + 3) \quad 4 \quad 5 \quad * \quad * \quad +$$
* $$1 \quad ((2 + 3) \times (4 \times 5)) \quad +$$
* $$(1 + ((2 + 3) \times (4 \times 5)))$$

> **Historical Note (Slide 9):** In 1972, Hewlett-Packard released the **HP-35**, the world's first handheld scientific calculator. It evaluated all expressions using RPN and a 4-level hardware stack. Pressing the famous `ENTER` key pushed values onto the stack, completely eliminating parentheses and instantly making slide rules obsolete!

---

### 2. The Postfix Evaluation Algorithm (Slide 10)

Because operands precede operators, evaluating postfix expressions with a **Stack** is exceptionally simple:

```
While input stream has tokens:
    Read token
    If token is a number (operand):
        Push number onto stack
    Else if token is an operator:
        Pop operand2
        Pop operand1
        Apply operator: result = operand1 (operator) operand2
        Push result onto stack

Final answer = pop()
```

#### Step-by-Step Evaluation Trace of `1 2 3 + 4 5 * * +` (Slide 10):

```
Token    Action                           Stack (Bottom -> Top)
-----------------------------------------------------------------
  1      Push 1                           [1.0]
  2      Push 2                           [1.0, 2.0]
  3      Push 3                           [1.0, 2.0, 3.0]
  +      Pop 3, Pop 2 -> 2 + 3 = 5        [1.0, 5.0]
  4      Push 4                           [1.0, 5.0, 4.0]
  5      Push 5                           [1.0, 5.0, 4.0, 5.0]
  *      Pop 5, Pop 4 -> 4 * 5 = 20       [1.0, 5.0, 20.0]
  *      Pop 20, Pop 5 -> 5 * 20 = 100    [1.0, 100.0]
  +      Pop 100, Pop 1 -> 1 + 100 = 101  [101.0]
```

Final result popped from stack: **`101.0`**

---

## Example C: Postfix Expression Evaluator (Slide 11)

Here is the complete implementation of the Postfix evaluation client from Slide 11:

```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.Scanner;

public class Postfix {
    public static void main(String[] args) {
        String input = "1 2 3 + 4 5 * * +";
        Scanner sc = new Scanner(input);

        // In Java, ArrayDeque is the standard modern implementation of a Stack
        Deque<Double> stack = new ArrayDeque<>();

        while (sc.hasNext()) {
            String token = sc.next();

            if (token.equals("*")) {
                stack.push(stack.pop() * stack.pop());
            } else if (token.equals("+")) {
                stack.push(stack.pop() + stack.pop());
            } else if (token.equals("-")) {
                // Note: subtraction is not commutative!
                // If stack has [a, b], b was popped first, then a.
                // a - b is equivalent to -b + a
                stack.push(-stack.pop() + stack.pop());
            } else if (token.equals("/")) {
                // Note: division is not commutative!
                // a / b is equivalent to (1 / b) * a
                stack.push(1.0 / stack.pop() * stack.pop());
            } else {
                stack.push(Double.parseDouble(token));
            }
        }

        System.out.println("Result: " + stack.pop());
        sc.close();
    }
}
```

### Critical Non-Commutative Pop Order Nuance:
Addition and multiplication are **commutative** ($$a + b = b + a$$ and $$a \times b = b \times a$$), so popping order does not alter the result.

However, **subtraction and division are NOT commutative**:
* When evaluating `8 2 /`:
  1. `2` is at the top of the stack. First `pop()` yields `2`.
  2. `8` is next. Second `pop()` yields `8`.
  3. The operation is $$8 / 2 = 4.0$$, **not** $$2 / 8 = 0.25$$!
* Writing it explicitly with two variables makes this mistake impossible to make:
  ```java
  double b = stack.pop(); // top element (second operand)
  double a = stack.pop(); // next element (first operand)
  stack.push(a / b);
  ```

---

## Task C: Extended Postfix Evaluator

Medium
{: .label .label-yellow }

Extend the basic postfix evaluator to support additional mathematical operators and robust input checking.

### Requirements:
1. Create a class named `PostfixEvaluator.java`.
2. Implement the method `public static double evaluate(String expr)`:
   * Support operators: `+`, `-`, `*`, `/`, `%` (modulo), and `^` (exponentiation via `Math.pow`).
   * For non-commutative operators (`-`, `/`, `%`, `^`), ensure `b` (top) and `a` (next) are popped and evaluated as `a op b`.
   * Support floating-point numeric tokens.
   * Return the final evaluated result.
3. Test your evaluator on the following expressions in `main`:
   * `"1 2 3 + 4 5 * * +"` $$\rightarrow$$ `101.0`
   * `"15 7 1 1 + - / 3 * 2 1 1 + + -"` $$\rightarrow$$ `5.0`
   * `"2 3 ^ 4 2 % +"` $$\rightarrow$$ `8.0`  *(Because $$2^3 + (4 \pmod 2) = 8 + 0 = 8$$)*

### Code Skeleton for `PostfixEvaluator.java`:
```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.Scanner;

public class PostfixEvaluator {

    public static double evaluate(String expr) {
        Scanner sc = new Scanner(expr);
        Deque<Double> stack = new ArrayDeque<>();

        while (sc.hasNext()) {
            String token = sc.next();

            // TODO 1: Handle addition (+)
            

            // TODO 2: Handle multiplication (*)
            

            // TODO 3: Handle subtraction (-) with correct operand order (a - b)
            

            // TODO 4: Handle division (/) with correct operand order (a / b)
            

            // TODO 5: Handle modulo (%) with correct operand order (a % b)
            

            // TODO 6: Handle exponentiation (^) using Math.pow(a, b)
            

            // TODO 7: Handle numeric operands (parse as Double and push)
            
        }

        sc.close();
        // TODO 8: Pop and return the final value on the stack
        return 0.0;
    }

    public static void main(String[] args) {
        String expr1 = "1 2 3 + 4 5 * * +";
        String expr2 = "15 7 1 1 + - / 3 * 2 1 1 + + -";
        String expr3 = "2 3 ^ 4 2 % +";

        // TODO 9: Evaluate and print the results for expr1, expr2, and expr3
        
    }
}
```

### Expected Output:
```
Expression 1: 101.0
Expression 2: 5.0
Expression 3: 8.0
```

---

## Part 5: Advanced Stack Application — Syntax Delimiter Validator

Hard
{: .label .label-red }

As noted in Slide 7, stacks are the foundational mechanism inside **compilers and interpreters**. 

One of the most important jobs of a Java compiler is ensuring that all brackets, braces, and parentheses are properly nested and balanced:
* `[ ( ) { [ ] } ]` $$\rightarrow$$ **Balanced**
* `[ ( ] )` $$\rightarrow$$ **Unbalanced** (mismatched delimiters)
* `{ ( ) } ]` $$\rightarrow$$ **Unbalanced** (extra closing bracket)
* `( ( ( )` $$\rightarrow$$ **Unbalanced** (unclosed delimiter)

### Delimiter Matching Algorithm:
1. Initialize an empty stack of characters: `Deque<Character> stack = new ArrayDeque<>()`.
2. Scan the string character by character:
   * If character is an opening delimiter (`'('`, `'{'`, `'['`), **push** it onto the stack.
   * If character is a closing delimiter (`')'`, `'}'`, `']'`):
     * If stack is empty, return `false` (no matching opening bracket).
     * **Pop** the top character from the stack.
     * Check if the popped opening bracket matches the closing bracket (e.g., `'('` matches `')'`). If not, return `false`.
3. After scanning the whole string, return `stack.isEmpty()` (if items remain on stack, some brackets were never closed).

---

## Task D: Syntax Delimiter Validator

Hard
{: .label .label-red }

Implement `DelimiterValidator.java` using the stack algorithm above.

### Instructions:
1. Implement the method `public static boolean isBalanced(String s)`:
   * Ignore any non-bracket characters (letters, numbers, spaces, operators).
   * Process all occurrences of `()`, `{}`, and `[]`.
   * Return `true` if balanced, `false` otherwise.
2. In `main`, test your implementation on the provided test cases.

### Code Skeleton for `DelimiterValidator.java`:
```java
import java.util.ArrayDeque;
import java.util.Deque;

public class DelimiterValidator {

    public static boolean isBalanced(String s) {
        Deque<Character> stack = new ArrayDeque<>();

        // TODO 1: Iterate over each character in string s
        

        // TODO 2: If character is '(', '{', or '[', push onto stack
        

        // TODO 3: If character is ')', '}', or ']'
        //   a. If stack is empty, return false
        //   b. Pop opening char from stack
        //   c. Check if opening and closing match. If mismatch, return false
        

        // TODO 4: Return true only if the stack is completely empty
        return false;
    }

    public static void main(String[] args) {
        String test1 = "public static void main(String[] args) { int[] a = {1, 2}; }";
        String test2 = "( [ { } ] )";
        String test3 = "( [ ) ]";
        String test4 = "{ ( ) } ]";
        String test5 = "( ( ( )";

        // TODO 5: Test and print the result for each test string
        
    }
}
```

### Expected Output:
```
test1: true
test2: true
test3: false
test4: false
test5: false
```

---

## Wrap-Up & Summary

By completing this lab, you have mastered:
1. **The Core Abstractions:** 
   * **Stack (LIFO):** Adding and removing from the same end (the top).
   * **Queue (FIFO):** Adding to the rear and removing from the front.
2. **Sequential Tracing:** Accurately predicting the step-by-step state of both structures on identical token streams.
3. **Dynamic Stream Ingestion:** Using a Queue as a flexible buffer to collect unknown numbers of items before allocating fixed-size arrays.
4. **Postfix / Reverse Polish Notation:** Why RPN requires no parentheses and how the HP-35 calculator revolutionized computing using a hardware stack.
5. **Stack-Based Computation:** Implementing postfix arithmetic evaluators and compiler delimiter balance validators.
