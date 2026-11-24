---
title: 2026csa
layout: page
nav_order: 2
parent: Computer Science A
---

# AP Computer Science A (2025–2026)
{: .no_toc }

Welcome to the course repository and lab guide for AP Computer Science A.

## Course Units & Labs
{: .no_toc .text-delta }
1. TOC
{:toc}

---

## Unit 1: Elementary Programming
* [Lab 1: Variables and Scope](lab1_variable.html)
* [Lab 2: Primitive Types](lab2_primitive.html)
* [Lab 3: Strings](lab3_string.html)
* [Lab 4: Arithmetic Expressions](lab4_expression.html)
* [Lab 5: Constants & Final](lab5_constants.html)
* [Lab 6: Method Signatures](lab6_method_sig.html)
* [Lab 6: Method Calling](lab6_method_calling.html)
* [Lab 7: Casting & Conversion](lab7_casting.html)
* [Lab 8: Using Objects](lab8_usingObject.html)
* [Lab 9: Creating Classes (Intro)](lab9_creatingClass.html)

---

## Unit 2: Selection & Iteration (If/Else & Loops)
* [Lab 10: Advanced String Algorithms](lab10_String.html)
* [Lab 11: Selection & If-Else Control Flow](lab11_ifElse.html)
* [Lab 12: Iteration & Loops](lab12_loops.html)

---

## Unit 3: Creating Classes
In Unit 3, students progress from *using* objects to *building complete classes from scratch* step-by-step:

* **[Unit 3 Lab 1: Class Anatomy & Encapsulation](unit3_lab1_class_anatomy.html)**
  * Class headers, private instance variables, immutability with `final`, accessor methods (getters), and mutator methods (setters).
  * Worked examples: `ProductReview.java` and `Fraction.java`.
  * Student implementation tasks: `ComplexNum.java` and `StepTracker.java`.
* **[Unit 3 Lab 2: Constructors & Object Initialization](unit3_lab2_constructors.html)**
  * Default constructors, parameterized constructors, constructor overloading, defensive argument validation, and chaining with `this(...)`.
  * Worked example: `Fraction.java`'s 4 constructors.
  * Student implementation tasks: `ComplexNum.java` constructor TODOs and `StepTracker.java` constructor.
* **[Unit 3 Lab 3: Instance Methods & State Mutation](unit3_lab3_instance_methods.html)**
  * Pure computational methods vs mutator methods, object creation inside methods returning new objects (`add`, `multiply`).
  * Worked examples: `Fraction.java` and `ComplexNum.java`.
  * Student implementation tasks: `ComplexNum.java` subtraction and division, and `StepTracker.java` AP CSA 2019 FRQ 1 methods (`activeDays`, `averageSteps`, `addDailySteps`).
* **[Unit 3 Lab 4: Capstone — 3D Virtual Gallery in Processing](unit3_lab4_virtual_gallery.html)**
  * Capstone project integrating **Units 1–3**: building an interactive 3D virtual art gallery with Processing (`P3D`) before advancing to Unit 4.
  * Encapsulates 2D wall paintings (`Painting`) and 3D floor sculptures (`Statue` with overloaded constructors, `.obj` models, rotating pedestals, and circular collision) connected via open, see-through `Doorway` stone archways without requiring arrays or `ArrayList`.
* **[Unit 3 Lab 4 Addendum: 3D Statue (`.obj`) & Painting Image Benchmarks](unit3_lab4_exhibition_guidelines.html)**
  * Empirical stress-test benchmarks (`20 rooms × 10 statues`), Processing `PShapeOBJ` & `PImage` memory/resolution limits, lazy-loading architecture for a 10-team see-through exhibition, and practical Blender `.obj` decimation/export guidelines.
* **[Unit 3 Lab 4 Setup: Processing 4 Installation & Git Basics](unit3_lab4_env_git_setup.html)**
  * Step-by-step Processing 4 download & installation guide (`4096 MB` memory setup) and beginner-friendly Git workflow (`clone`, `checkout -b`, `status`, `add`, `commit`, `push`, `pull`, `restore`) for team SDLC collaboration.

---

## Unit 4: Data Collection (Upcoming)
* 1D Arrays, `ArrayList`, 2D Arrays, and standard dataset traversal algorithms.
