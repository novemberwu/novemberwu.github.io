---
title: Double Linked List Lab
nav_order: 11
parent: DS Labs
layout: page

---
# Double Linked List Lab
{: .no_toc}


## Goal
{: .no_toc}
* Gain an understanding of the usage of a data structure backing linked list
* Have experience with building a middle-size project from the scratch

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Delivery
Your delivery is the java file. Please make sure uploading the source files (.java) files.

## Software Engineering Philosophy
{: .no_toc}
A common beginner mistake in software engineering is to write a large amount of code 
and hope that it all work once you've finished. This makes life very difficult for a programmer.

A better approach is breaking down a whole piece of program into many smaller parts. Each time I make a 
baby step by modifying a small part, then verify that small piece works as expected. then go another baby step, so on and on.

To help encourage better programming habits, in this lab, we are going to hold your hands through the development
process. 

{: .note }
For this lab, you must work alone. Please do not use ```java.util``` data structures
in your implementation. The whole point is to build your own version of linked list

## Task A: Set up code skeleton (5% ponits)
Follow the [Guide](https://github.com/XiaochuanQian/PostAPDS-Lab-doubleLinkedList) to get the
code skeleton. The code skeleton defines the interfaces of Double Linked List

## Task B: Write and verify ```addFirst``` and ```addLast``` (25% points)
```addFirst``` and ```addLast``` *may not* use looping or recursion. A single add operation must take "constant time"
that is, adding an element should take approximately the same amount of time regardless the length of the list.

you can use the debugger or java visualizer to verify that your code is working correctly

## Task C: Write and verify ```getFirst``` and ```getLast``` (25% points)
Those are operations to return the value of the first node and last node of the list

## Task D: Write and verify constructors ```DLList()``` and ```DLList(T x)``` (10% points)
As  constructor ```DLList()``` will make an empty list, you might want to revise your implementation to make all methods
null safe.

## Task E: Write and verify ```removeFirst()``` and ```removeLast()``` (25% points)
```removeFirst``` and ```removeLast``` *may not* use looping or recursion. A single remove operation must take "constant time"
that is, removing an element should take approximately the same amount of time regardless the length of the list.

## Task F: Write ```size``` (5% points)
size is the number of nodes in the list. if you use sentinel nodes, they don't count. 
Because they are the implementation detail, not something we should expose to the user

## Task G: Write and verify ```get()``` (5% points)
You may use the loop or recursion to implement get, as there is no constant time implementation.

## Task H: Adding Test Cases ( Optional 10% points)
Write test cases and show the test coverage of your source. Please make sure test cases contain 3 parts
1. Precondition, or setup for the test
2. Execution of the to-be-tested functions
3. Assertion on expected result and actual result

### Test Code: Correctness and Efficiency

You can use the following test class (`DLListTest.java`) to test both the **correctness** and the **efficiency** (constant time O(1) performance) of your implementation. It uses pure standard Java and can be executed directly without any external libraries.

```java
public class DLListTest {

    private static int passedTests = 0;
    private static int failedTests = 0;

    public static void main(String[] args) {
        System.out.println("========================================================");
        System.out.println("            DLList Correctness Tests                    ");
        System.out.println("========================================================");

        testDefaultConstructor();
        testSingleItemConstructor();
        testAddFirst();
        testAddLast();
        testMixedAdd();
        testRemoveFirst();
        testRemoveLast();
        testInterleavedAddRemove();
        testAddAfterEmpty();
        testGetIndex();
        testGenerics();

        System.out.println();
        System.out.println("========================================================");
        System.out.println("        DLList Efficiency (Time Complexity) Tests       ");
        System.out.println("========================================================");

        testEfficiencyAddFirst();
        testEfficiencyAddLast();
        testEfficiencyRemoveFirst();
        testEfficiencyRemoveLast();
        testEfficiencySize();

        System.out.println();
        System.out.println("========================================================");
        System.out.printf("Results: %d Passed, %d Failed%n", passedTests, failedTests);
        System.out.println("========================================================");

        if (failedTests > 0) {
            System.exit(1);
        }
    }

    // -------------------------------------------------------------------------
    // Helper Assertion Methods
    // -------------------------------------------------------------------------

    private static void assertTrue(String testName, boolean condition, String failureMsg) {
        if (condition) {
            System.out.println("  [PASS] " + testName);
            passedTests++;
        } else {
            System.out.println("  [FAIL] " + testName + ": " + failureMsg);
            failedTests++;
        }
    }

    private static <E> void assertEquals(String testName, E expected, E actual) {
        boolean match = (expected == null && actual == null) || (expected != null && expected.equals(actual));
        if (match) {
            System.out.println("  [PASS] " + testName);
            passedTests++;
        } else {
            System.out.println("  [FAIL] " + testName + " -> Expected: " + expected + ", Actual: " + actual);
            failedTests++;
        }
    }

    // -------------------------------------------------------------------------
    // Part 1: Correctness Tests (Testing edge cases and pointer operations)
    // -------------------------------------------------------------------------

    public static void testDefaultConstructor() {
        System.out.println("- Testing Default Constructor DLList():");
        // 1. Precondition
        DLList<Integer> list = new DLList<>();
        // 2 & 3. Execution & Assertion
        assertEquals("Empty list size should be 0", 0, list.size());
        assertEquals("Empty list getFirst() should be null", null, list.getFirst());
        assertEquals("Empty list getLast() should be null", null, list.getLast());
    }

    public static void testSingleItemConstructor() {
        System.out.println("- Testing Single-Item Constructor DLList(x):");
        // 1. Precondition & 2. Execution
        DLList<Integer> list = new DLList<>(42);
        // 3. Assertion
        assertEquals("List size should be 1", 1, list.size());
        assertEquals("getFirst() should return 42", Integer.valueOf(42), list.getFirst());
        assertEquals("getLast() should return 42", Integer.valueOf(42), list.getLast());
        assertEquals("get(0) should return 42", Integer.valueOf(42), list.get(0));
    }

    public static void testAddFirst() {
        System.out.println("- Testing addFirst(x):");
        DLList<Integer> list = new DLList<>();
        list.addFirst(10);
        list.addFirst(20);
        list.addFirst(30); // [30, 20, 10]

        assertEquals("Size after 3 addFirst calls should be 3", 3, list.size());
        assertEquals("getFirst() should be 30", Integer.valueOf(30), list.getFirst());
        assertEquals("getLast() should be 10", Integer.valueOf(10), list.getLast());
        assertEquals("Index 0 should be 30", Integer.valueOf(30), list.get(0));
        assertEquals("Index 1 should be 20", Integer.valueOf(20), list.get(1));
        assertEquals("Index 2 should be 10", Integer.valueOf(10), list.get(2));
    }

    public static void testAddLast() {
        System.out.println("- Testing addLast(x):");
        DLList<Integer> list = new DLList<>();
        list.addLast(10);
        list.addLast(20);
        list.addLast(30); // [10, 20, 30]

        assertEquals("Size after 3 addLast calls should be 3", 3, list.size());
        assertEquals("getFirst() should be 10", Integer.valueOf(10), list.getFirst());
        assertEquals("getLast() should be 30", Integer.valueOf(30), list.getLast());
        assertEquals("Index 0 should be 10", Integer.valueOf(10), list.get(0));
        assertEquals("Index 1 should be 20", Integer.valueOf(20), list.get(1));
        assertEquals("Index 2 should be 30", Integer.valueOf(30), list.get(2));
    }

    public static void testMixedAdd() {
        System.out.println("- Testing Mixed addFirst and addLast:");
        DLList<Integer> list = new DLList<>();
        list.addLast(20);   // [20]
        list.addFirst(10);  // [10, 20]
        list.addLast(30);   // [10, 20, 30]
        list.addFirst(5);   // [5, 10, 20, 30]

        assertEquals("Size should be 4", 4, list.size());
        assertEquals("getFirst() should be 5", Integer.valueOf(5), list.getFirst());
        assertEquals("getLast() should be 30", Integer.valueOf(30), list.getLast());
        assertEquals("get(0) should be 5", Integer.valueOf(5), list.get(0));
        assertEquals("get(1) should be 10", Integer.valueOf(10), list.get(1));
        assertEquals("get(2) should be 20", Integer.valueOf(20), list.get(2));
        assertEquals("get(3) should be 30", Integer.valueOf(30), list.get(3));
    }

    public static void testRemoveFirst() {
        System.out.println("- Testing removeFirst():");
        DLList<Integer> list = new DLList<>();
        list.addLast(10);
        list.addLast(20);
        list.addLast(30); // [10, 20, 30]

        Integer r1 = list.removeFirst();
        assertEquals("First removeFirst() should return 10", Integer.valueOf(10), r1);
        assertEquals("Size after 1 removal should be 2", 2, list.size());
        assertEquals("New getFirst() should be 20", Integer.valueOf(20), list.getFirst());

        Integer r2 = list.removeFirst();
        assertEquals("Second removeFirst() should return 20", Integer.valueOf(20), r2);
        assertEquals("Size after 2 removals should be 1", 1, list.size());
        assertEquals("getLast() should be 30", Integer.valueOf(30), list.getLast());

        Integer r3 = list.removeFirst();
        assertEquals("Third removeFirst() should return 30", Integer.valueOf(30), r3);
        assertEquals("List should now be empty (size 0)", 0, list.size());
        assertEquals("getFirst() on emptied list should be null", null, list.getFirst());
        assertEquals("getLast() on emptied list should be null", null, list.getLast());
    }

    public static void testRemoveLast() {
        System.out.println("- Testing removeLast():");
        DLList<Integer> list = new DLList<>();
        list.addLast(10);
        list.addLast(20);
        list.addLast(30); // [10, 20, 30]

        Integer r1 = list.removeLast();
        assertEquals("First removeLast() should return 30", Integer.valueOf(30), r1);
        assertEquals("Size after 1 removal should be 2", 2, list.size());
        assertEquals("New getLast() should be 20", Integer.valueOf(20), list.getLast());

        Integer r2 = list.removeLast();
        assertEquals("Second removeLast() should return 20", Integer.valueOf(20), r2);
        assertEquals("Size after 2 removals should be 1", 1, list.size());
        assertEquals("getLast() should be 10", Integer.valueOf(10), list.getLast());

        Integer r3 = list.removeLast();
        assertEquals("Third removeLast() should return 10", Integer.valueOf(10), r3);
        assertEquals("List should now be empty (size 0)", 0, list.size());
        assertEquals("getFirst() on emptied list should be null", null, list.getFirst());
        assertEquals("getLast() on emptied list should be null", null, list.getLast());
    }

    public static void testInterleavedAddRemove() {
        System.out.println("- Testing Interleaved Adds and Removes (Bidirectional pointer check):");
        DLList<Integer> list = new DLList<>();
        list.addFirst(1); // [1]
        list.addLast(2);  // [1, 2]
        list.addLast(3);  // [1, 2, 3]
        assertEquals("removeFirst should be 1", Integer.valueOf(1), list.removeFirst()); // [2, 3]
        list.addFirst(0); // [0, 2, 3]
        assertEquals("removeLast should be 3", Integer.valueOf(3), list.removeLast());   // [0, 2]
        assertEquals("getFirst should be 0", Integer.valueOf(0), list.getFirst());
        assertEquals("getLast should be 2", Integer.valueOf(2), list.getLast());
        assertEquals("Size should be 2", 2, list.size());
    }

    public static void testAddAfterEmpty() {
        System.out.println("- Testing Add after Emptying (Sentinel/Pointer Reset Check):");
        DLList<Integer> list = new DLList<>();
        list.addFirst(100);
        list.removeFirst();
        assertEquals("List is empty", 0, list.size());

        // Re-adding after list was completely emptied
        list.addFirst(200);
        list.addLast(300);
        assertEquals("Size after re-add should be 2", 2, list.size());
        assertEquals("getFirst() should be 200", Integer.valueOf(200), list.getFirst());
        assertEquals("getLast() should be 300", Integer.valueOf(300), list.getLast());
    }

    public static void testGetIndex() {
        System.out.println("- Testing get(int i) and Boundary Checks:");
        DLList<Integer> list = new DLList<>();
        for (int i = 0; i < 5; i++) {
            list.addLast(i * 10); // [0, 10, 20, 30, 40]
        }
        assertEquals("get(0) should be 0", Integer.valueOf(0), list.get(0));
        assertEquals("get(2) should be 20", Integer.valueOf(20), list.get(2));
        assertEquals("get(4) should be 40", Integer.valueOf(40), list.get(4));
        assertEquals("get(-1) out-of-bounds should be null", null, list.get(-1));
        assertEquals("get(5) out-of-bounds should be null", null, list.get(5));
    }

    public static void testGenerics() {
        System.out.println("- Testing Generic Type Support DLList<String>:");
        DLList<String> stringList = new DLList<>();
        stringList.addLast("Data");
        stringList.addLast("Structures");
        stringList.addFirst("AP");

        assertEquals("Size should be 3", 3, stringList.size());
        assertEquals("First string should be 'AP'", "AP", stringList.getFirst());
        assertEquals("Last string should be 'Structures'", "Structures", stringList.getLast());
    }

    // -------------------------------------------------------------------------
    // Part 2: Efficiency (Time Complexity) Tests
    // Verifies that addFirst, addLast, removeFirst, removeLast, and size() take
    // constant time O(1) rather than O(N).
    // -------------------------------------------------------------------------

    private static final int OP_COUNT = 100_000;
    private static final long MAX_TIME_MS = 1_000; // 1-second threshold for O(1) operations

    public static void testEfficiencyAddFirst() {
        System.out.println("- Benchmark: 100,000 addFirst operations (Expected: O(1) constant time):");
        DLList<Integer> list = new DLList<>();
        long start = System.currentTimeMillis();
        for (int i = 0; i < OP_COUNT; i++) {
            list.addFirst(i);
        }
        long elapsed = System.currentTimeMillis() - start;

        assertTrue(String.format("100,000 addFirst calls completed in %d ms (Threshold: %d ms)", elapsed, MAX_TIME_MS),
                elapsed < MAX_TIME_MS && list.size() == OP_COUNT,
                String.format("addFirst took %d ms (too slow, likely looping or recursive O(N))", elapsed));
    }

    public static void testEfficiencyAddLast() {
        System.out.println("- Benchmark: 100,000 addLast operations (Expected: O(1) constant time):");
        DLList<Integer> list = new DLList<>();
        long start = System.currentTimeMillis();
        for (int i = 0; i < OP_COUNT; i++) {
            list.addLast(i);
        }
        long elapsed = System.currentTimeMillis() - start;

        assertTrue(String.format("100,000 addLast calls completed in %d ms (Threshold: %d ms)", elapsed, MAX_TIME_MS),
                elapsed < MAX_TIME_MS && list.size() == OP_COUNT,
                String.format("addLast took %d ms! Did you traverse the list to find the end instead of using sentinel.prev or tail in O(1)?", elapsed));
    }

    public static void testEfficiencyRemoveFirst() {
        System.out.println("- Benchmark: 100,000 removeFirst operations (Expected: O(1) constant time):");
        DLList<Integer> list = new DLList<>();
        for (int i = 0; i < OP_COUNT; i++) {
            list.addFirst(i);
        }

        long start = System.currentTimeMillis();
        for (int i = 0; i < OP_COUNT; i++) {
            list.removeFirst();
        }
        long elapsed = System.currentTimeMillis() - start;

        assertTrue(String.format("100,000 removeFirst calls completed in %d ms (Threshold: %d ms)", elapsed, MAX_TIME_MS),
                elapsed < MAX_TIME_MS && list.size() == 0,
                String.format("removeFirst took %d ms (too slow)", elapsed));
    }

    public static void testEfficiencyRemoveLast() {
        System.out.println("- Benchmark: 100,000 removeLast operations (Expected: O(1) constant time):");
        DLList<Integer> list = new DLList<>();
        for (int i = 0; i < OP_COUNT; i++) {
            list.addFirst(i);
        }

        long start = System.currentTimeMillis();
        for (int i = 0; i < OP_COUNT; i++) {
            list.removeLast();
        }
        long elapsed = System.currentTimeMillis() - start;

        assertTrue(String.format("100,000 removeLast calls completed in %d ms (Threshold: %d ms)", elapsed, MAX_TIME_MS),
                elapsed < MAX_TIME_MS && list.size() == 0,
                String.format("removeLast took %d ms! In a DLList, removeLast MUST be O(1) using prev pointers. Traversal from front is O(N)!", elapsed));
    }

    public static void testEfficiencySize() {
        System.out.println("- Benchmark: 1,000,000 size() queries on large list (Expected: O(1) cached size):");
        DLList<Integer> list = new DLList<>();
        for (int i = 0; i < OP_COUNT; i++) {
            list.addLast(i);
        }

        long start = System.currentTimeMillis();
        int s = 0;
        for (int i = 0; i < 1_000_000; i++) {
            s += list.size();
        }
        long elapsed = System.currentTimeMillis() - start;

        assertTrue(String.format("1,000,000 size() calls completed in %d ms (Threshold: 300 ms)", elapsed),
                elapsed < 300 && s > 0,
                String.format("size() took %d ms! Did you count nodes with a loop instead of maintaining a size instance variable in O(1)?", elapsed));
    }
}
```
