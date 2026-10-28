---
title: Lab 12 Loops & String Algorithms
layout: page
nav_order: 12
parent: 2026csa
math: mathjax
---

# Lab 12: Iteration & String Traversal Algorithms
{: .no_toc }

## Goals
{: .no_toc}
* Understand the anatomy and execution flow of `for` and `while` loops in Java.
* Master forward and reverse **linear traversal** of Strings using 0-based indexing and `substring(i, i + 1)`.
* Apply the **accumulator pattern** to count characters and occurrences.
* Analyze multi-character **sliding window** algorithms and avoid common off-by-one boundary traps (`StringIndexOutOfBoundsException`).
* Contrast **overlapping** versus **non-overlapping** pattern matching and learn when to use adaptive index increments.

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Part 1: Anatomy of a `for` Loop & String Traversal

In Java, iteration allows a program to execute a block of code repeatedly. For definite loops where the number of repetitions is known in advance, the `for` loop is the standard construct:

```java
for (initialization; condition; update) {
    // Loop body statements
}
```

```
           1. Initialization (runs once)
                       │
                       ▼
         ┌───► 2. Evaluate Condition ─────── false ──────┐
         │             │                                 │
         │             │ true                            │
         │             ▼                                 │
         │     3. Execute Loop Body                      │
         │             │                                 │
         │             ▼                                 │
         └───  4. Execute Update                         ▼
                                                  Loop Terminates
```

### 0-Based Indexing and Character Traversal

Every character in a Java `String` is addressed by an integer index from `0` to `s.length() - 1`. In AP CSA, the standard idiom for extracting a single character at index `i` as a `String` is:

```java
String letter = s.substring(i, i + 1);
```

```
String s = "JAVA";

Index:       0     1     2     3
Character:  'J'   'A'   'V'   'A'
             ▲                 ▲
             │                 │
         First Index       Last Index
           (i = 0)     (i = s.length() - 1)
```

### Forward Traversal: `walk(String s)`

The `walk` method visits every character from the beginning of the string to the end:

```java
public static void walk(String s) {
    for (int i = 0; i < s.length(); i++) {
        System.out.println(s.substring(i, i + 1));
    }
}
```

* **Start:** `int i = 0` (the first character).
* **Stop:** `i < s.length()` (stops when `i == s.length()`, ensuring the last index processed is `s.length() - 1`).
* **Step:** `i++` (advances forward by 1).

---

## Task A: Reverse String Traversal (`walkReversely`)

Easy
{: .label .label-green }

Implement `walkReversely(String s)` (**TODO#1**) to print each character of the string on its own line, starting from the **last character** down to the **first character**.

### Key Mechanics:
1. **Starting Index:** The last valid index is `s.length() - 1`.
2. **Continuation Condition:** The loop must continue running as long as `i >= 0`.
3. **Step / Decrement:** On each step, decrement the counter: `i--`.
4. **Extraction:** Inside the loop, extract `s.substring(i, i + 1)` and print it with `System.out.println(...)`.

{: .warning }
> **AP Exam Classic Traps:**
> * Starting at `i = s.length()` causes a runtime `StringIndexOutOfBoundsException` because `s.substring(s.length(), s.length() + 1)` attempts to access an index beyond the string length!
> * Using the condition `i > 0` instead of `i >= 0` accidentally stops before printing the first character at index `0`!

### Code Skeleton:
```java
public static void walkReversely(String s) {
    // TODO#1: Loop backwards from s.length() - 1 down to 0
}
```

### Example:
Calling `walkReversely("Java")` must print:
```
a
v
a
J
```

---

## Part 2: The Accumulator Pattern & Filtering

A common algorithmic pattern across AP CSA is the **accumulator pattern**:
1. Initialize a counter or running total variable before the loop (`int count = 0;`).
2. Loop over each element in the sequence.
3. Test a condition using an `if` statement (filtering).
4. If the condition holds true, increment the accumulator (`count++;`).
5. After the loop finishes, return or use the accumulated result.

---

## Task B: Counting Vowels (`countVowels`)

Easy
{: .label .label-green }

Implement `countVowels(String s)` (**TODO#2**) to count and return the total number of vowels (`a`, `e`, `i`, `o`, `u`, case-insensitive) present in string `s`.

### Instructions:
1. Inspect the provided method signature and skeleton:
   ```java
   public static int countVowels(String s) {
       int count = 0;
       for (int i = 0; i < s.length(); i++) {
           System.out.println(s.substring(i, i + 1));
           // TODO#2 determine the letter is one of the vowel letter
       }
       return count;
   }
   ```
2. For each iteration, extract the current single-character substring:
   ```java
   String letter = s.substring(i, i + 1);
   ```
3. Check whether `letter` is a vowel. You can choose any of the following valid AP CSA approaches:
   * **Approach 1 (Case Conversion + Logical OR):**
     ```java
     String lower = letter.toLowerCase();
     if (lower.equals("a") || lower.equals("e") || lower.equals("i") ||
         lower.equals("o") || lower.equals("u")) {
         count++;
     }
     ```
   * **Approach 2 (`indexOf` lookup):**
     ```java
     if ("aeiouAEIOU".indexOf(letter) != -1) {
         count++;
     }
     ```
   * **Approach 3 (`equalsIgnoreCase`):**
     ```java
     if (letter.equalsIgnoreCase("a") || letter.equalsIgnoreCase("e") ||
         letter.equalsIgnoreCase("i") || letter.equalsIgnoreCase("o") ||
         letter.equalsIgnoreCase("u")) {
         count++;
     }
     ```
4. Return `count`. *(Note: You may keep or remove the debug `System.out.println` line as desired).*

### Example:
* `countVowels("Computer Science")` $$\rightarrow$$ returns `6` (`o`, `u`, `e`, `i`, `e`, `e`).
* `countVowels("AP CSA")` $$\rightarrow$$ returns `2` (`A`, `A`).
* `countVowels("rhythm")` $$\rightarrow$$ returns `0`.

---

## Part 3: Sliding Window & The Loop Boundary Trap

When searching for multi-character substrings (e.g., target length $$k > 1$$), we examine a **sliding window** of width `target.length()` across the string.

```
Searching for target = "cat" (length = 3) in s = "scatter" (length = 7):

 i=0: [s c a] t t e r  --> "sca" (no)
 i=1:  s [c a t] t e r --> "cat" (MATCH!)
 i=2:  s c [a t t] e r --> "att" (no)
 i=3:  s c a [t t e] r --> "tte" (no)
 i=4:  s c a t [t e r] --> "ter" (no)
```

At what index $$i$$ must the sliding window stop?
* The last valid window starts at index: `s.length() - target.length()`
* For `"scatter"` ($$7$$) and `"cat"` ($$3$$), the last starting index is $$7 - 3 = 4$$. Window: `s.substring(4, 4 + 3)` = `s.substring(4, 7)` = `"ter"`.

### The Boundary Trap in `countOccurrences`

Now look closely at the provided `countOccurrences` method:

```java
public static int countOccurrences(String s, String target) {
    int count = 0;
    for (int i = 0; i < s.length() - target.length(); i++) {
        String w = s.substring(i, i + target.length());
        if (w.equals(target)) count++;
    }

    return count;
}
```

{: .highlight }
> ### 🚨 Bug Analysis: Why does `i < s.length() - target.length()` fail?
>
> 1. **Case: Target at the very end of String**
>    Suppose `s = "banana"` (length 6) and `target = "na"` (length 2).
>    * `s.length() - target.length()` = $$6 - 2 = 4$$.
>    * The loop condition `i < 4` allows $$i = 0, 1, 2, 3$$.
>    * When $$i = 4$$, the condition `4 < 4` is **`false`**, so the loop terminates!
>    * But index 4 is the substring `s.substring(4, 6)` = `"na"`, which is a target match! The method misses it!
>
> 2. **Case: Equal lengths (`s` equals `target`)**
>    Suppose `s = "cat"` and `target = "cat"`.
>    * `s.length() - target.length()` = $$3 - 3 = 0$$.
>    * The loop evaluates `i < 0` at $$i = 0$$, which is immediately **`false`**! The loop body never executes, returning `0` instead of `1`!
>
> 3. **The Fix:**
>    To include the final window, the loop condition must be:
>    ```java
>    i <= s.length() - target.length()
>    ```
>    or equivalently:
>    ```java
>    i < s.length() - target.length() + 1
>    ```

### Overlapping Matches

Notice that `countOccurrences` with `i++` counts **overlapping occurrences**:

```
s = "aaaa", target = "aa"

i=0: [a a] a a  --> match (count = 1)
i=1:  a [a a] a --> match (count = 2)
i=2:  a a [a a] --> match (count = 3)

Total = 3
```

Because `i` increases by 1 on every step regardless of whether a match occurred, the second `"aa"` shares an `'a'` with the first `"aa"`.

---

## Part 4: Non-Overlapping Search & Adaptive Stepping

In many real-world applications (such as token parsing, DNA motif counting, and text find-and-replace), matches **must not overlap**.

```
s = "aaaa", target = "aa" (Non-overlapping)

Window 1 at index 0: [a a] a a  --> MATCH! (count = 1)
Skip past matched target! Advance i by target.length() (i = 0 + 2 = 2)

Window 2 at index 2:  a a [a a] --> MATCH! (count = 2)
Skip past matched target! Advance i by target.length() (i = 2 + 2 = 4)

Total = 2
```

Another example:
* `s = "banana"`, `target = "ana"`:
  * Index 1: `"ana"` (match! advance past index 1 + 3 = 4)
  * Index 4: `"na"` is too short to form another `"ana"`
  * Non-overlapping count = `1` (whereas overlapping count = `2`, at index 1 and index 3).

---

## Task C: Non-Overlapping Occurrences (`countOccurrencesWithoutOverlapping`)

Medium
{: .label .label-yellow }

Implement `countOccurrencesWithoutOverlapping(String s, String target)` (**TODO#3**) to count how many times `target` appears in `s` without counting overlapping characters.

### Designing the Solution: `for` vs. `while`

In a standard `for` loop, the update expression `i++` always increments by 1 on every step. But here:
* When a match is **found**, we want to advance `i` by `target.length()` (jumping past the matched characters).
* When there is **no match**, we advance `i` by `1` to test the next starting character.

Because the increment varies dynamically, a `while` loop is the natural and most readable tool!

### Approach 1: `while` Loop with Substring Window
```java
public static int countOccurrencesWithoutOverlapping(String s, String target) {
    if (target.length() == 0 || target.length() > s.length()) {
        return 0;
    }

    int count = 0;
    int i = 0;

    while (i <= s.length() - target.length()) {
        String w = s.substring(i, i + target.length());
        if (w.equals(target)) {
            count++;
            i += target.length(); // Jump past the entire matched target!
        } else {
            i++;                  // Shift by 1 character
        }
    }

    return count;
}
```

### Approach 2: `while` Loop with `indexOf(str, fromIndex)`
Java's `String.indexOf(String target, int fromIndex)` returns the next occurrence starting at or after `fromIndex`:
```java
public static int countOccurrencesWithoutOverlapping(String s, String target) {
    if (target.length() == 0) {
        return 0;
    }

    int count = 0;
    int index = s.indexOf(target);

    while (index != -1) {
        count++;
        // Resume searching immediately after the current match
        index = s.indexOf(target, index + target.length());
    }

    return count;
}
```

---

## Complete Starter & Test Code

Create a file named `LoopStringExamples.java` with the full class implementation and test suite in `main`:

```java
public class LoopStringExamples {

    // 1. Walk through the string forward, printing each character
    public static void walk(String s) {
        for (int i = 0; i < s.length(); i++) {
            System.out.println(s.substring(i, i + 1));
        }
    }

    // 2. Walk through the string backward, printing each character
    public static void walkReversely(String s) {
        // TODO#1: Loop from s.length() - 1 down to 0
        for (int i = s.length() - 1; i >= 0; i--) {
            System.out.println(s.substring(i, i + 1));
        }
    }

    // 3. Count total vowels in the string
    public static int countVowels(String s) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            String letter = s.substring(i, i + 1);
            // TODO#2: Determine if letter is a vowel (a, e, i, o, u)
            if ("aeiouAEIOU".indexOf(letter) != -1) {
                count++;
            }
        }
        return count;
    }

    // 4. Count overlapping occurrences of target in s
    public static int countOccurrences(String s, String target) {
        int count = 0;
        // Notice the loop bound: <= ensures the final substring is inspected!
        for (int i = 0; i <= s.length() - target.length(); i++) {
            String w = s.substring(i, i + target.length());
            if (w.equals(target)) {
                count++;
            }
        }
        return count;
    }

    // 5. Count non-overlapping occurrences of target in s
    public static int countOccurrencesWithoutOverlapping(String s, String target) {
        // TODO#3: Skip target.length() when a match is found
        if (target.length() == 0 || target.length() > s.length()) {
            return 0;
        }

        int count = 0;
        int i = 0;
        while (i <= s.length() - target.length()) {
            String w = s.substring(i, i + target.length());
            if (w.equals(target)) {
                count++;
                i += target.length(); // Advance past the match
            } else {
                i++;                  // Advance by 1
            }
        }
        return count;
    }

    // Test Client
    public static void main(String[] args) {
        System.out.println("=== Test 1: walk(\"Java\") ===");
        walk("Java");

        System.out.println("\n=== Test 2: walkReversely(\"Java\") ===");
        walkReversely("Java");

        System.out.println("\n=== Test 3: countVowels ===");
        System.out.println("Vowels in 'Computer Science': " + countVowels("Computer Science")); // 6
        System.out.println("Vowels in 'AP CSA': " + countVowels("AP CSA"));                     // 2
        System.out.println("Vowels in 'rhythm': " + countVowels("rhythm"));                     // 0

        System.out.println("\n=== Test 4: countOccurrences (Overlapping) ===");
        System.out.println("Occurrences of 'na' in 'banana': " + countOccurrences("banana", "na")); // 2
        System.out.println("Occurrences of 'aa' in 'aaaa': " + countOccurrences("aaaa", "aa"));     // 3
        System.out.println("Occurrences of 'cat' in 'cat': " + countOccurrences("cat", "cat"));   // 1

        System.out.println("\n=== Test 5: countOccurrencesWithoutOverlapping ===");
        System.out.println("Non-overlapping 'aa' in 'aaaa': " + countOccurrencesWithoutOverlapping("aaaa", "aa"));     // 2
        System.out.println("Non-overlapping 'ana' in 'banana': " + countOccurrencesWithoutOverlapping("banana", "ana")); // 1
        System.out.println("Non-overlapping 'ab' in 'abcde': " + countOccurrencesWithoutOverlapping("abcde", "ab"));   // 1
        System.out.println("Non-overlapping 'xyz' in 'abcde': " + countOccurrencesWithoutOverlapping("abcde", "xyz")); // 0
    }
}
```

---

## Expected Output

```
=== Test 1: walk("Java") ===
J
a
v
a

=== Test 2: walkReversely("Java") ===
a
v
a
J

=== Test 3: countVowels ===
Vowels in 'Computer Science': 6
Vowels in 'AP CSA': 2
Vowels in 'rhythm': 0

=== Test 4: countOccurrences (Overlapping) ===
Occurrences of 'na' in 'banana': 2
Occurrences of 'aa' in 'aaaa': 3
Occurrences of 'cat' in 'cat': 1

=== Test 5: countOccurrencesWithoutOverlapping ===
Non-overlapping 'aa' in 'aaaa': 2
Non-overlapping 'ana' in 'banana': 1
Non-overlapping 'ab' in 'abcde': 1
Non-overlapping 'xyz' in 'abcde': 0
```

---

## Summary of AP CSA String Loop Rules

| Pattern | Loop Structure | Boundary / Condition | Common Pitfall |
| :--- | :--- | :--- | :--- |
| **Forward Traversal** | `for (int i = 0; i < s.length(); i++)` | `0` to `s.length() - 1` | Using `<= s.length()` causes `StringIndexOutOfBoundsException`. |
| **Backward Traversal** | `for (int i = s.length() - 1; i >= 0; i--)` | `s.length() - 1` down to `0` | Starting at `s.length()` causes out-of-bounds error; using `i > 0` misses index `0`. |
| **Sliding Window (len $$k$$)** | `for (int i = 0; i <= s.length() - k; i++)` | `0` to `s.length() - k` | Using `< s.length() - k` skips the last window at the end of the string. |
| **Non-Overlapping Window** | `while (i <= s.length() - k)` | If match: `i += k`<br>Else: `i++` | Incrementing `i++` every time erroneously counts overlapping matches. |
