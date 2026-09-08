---
title: Rate of Change and Concavity
nav_order: 1
parent: Pre-AP Algebra
layout: page

---

# Grade 9 Algebra 2: Rate of Change and Concavity
**Curriculum Unit:** Functions and Their Graphs  
**Section:** Rate of Change and Concavity  

---

## Part 1: In-Class Guided Worksheet 

**Name:** ____________________________________ **Date:** _______________ **Period:** ______

### Learning Objectives
1. **Objective 1:** Calculate and interpret the Average Rate of Change (AROC) of a function algebraically and graphically as the slope of a secant line.
2. **Objective 2:** Determine intervals of concavity (concave upward / concave downward) using consecutive average rates of change and tabular data.
3. **Objective 3:** Identify points of inflection graphically, tabularly, and algebraically, and explain the geometric relationship between the curve and its secant/tangent lines.

---

### Reference Review & Key Formulas

> **Definition (Average Rate of Change):**  
> If $$f$$ is a function defined on $$[a, b]$$, the average rate of change from $$a$$ to $$b$$ is:
> $$\text{AROC} = \frac{\Delta y}{\Delta x} = \frac{f(b) - f(a)}{b - a}, \quad b \neq a$$
> Geometrically, this represents the slope of the **secant line** through $$(a, f(a))$$ and $$(b, f(b))$$.

> **Theorem 1 (Rate of Change Monotonicity & Concavity):**  
> Over consecutive intervals of equal width $$\Delta x = h$$:
> - If $$\text{AROC}$$ is **strictly increasing** ($$r_1 < r_2 < r_3$$), the graph is **concave upward** ("holds water").
> - If $$\text{AROC}$$ is **strictly decreasing** ($$r_1 > r_2 > r_3$$), the graph is **concave downward** ("spills water").

> **Definition (Point of Inflection):**  
> A point $$(c, f(c))$$ on a continuous curve where the concavity changes from upward to downward or from downward to upward. At this point, the tangent line crosses through the curve.

---

### Objective 1: Average Rate of Change (AROC) & Secant Lines

#### Exercise 1 (Guided Exploration)
Given the function $$f(x) = 2x^2 - 3x + 1$$:
1. Compute the average rate of change on $$[1, 3]$$.
    - $$f(1) =$$ ____________
    - $$f(3) =$$ ____________
    - $$\text{AROC}_{[1, 3]} = \frac{f(3) - f(1)}{3 - 1} =$$ __________________

2. Compute the average rate of change on $$[3, 5]$$.
    - $$f(5) =$$ ____________
    - $$\text{AROC}_{[3, 5]} = \frac{f(5) - f(3)}{5 - 3} =$$ __________________

3. Write the point-slope equation of the secant line passing through $$(1, f(1))$$ and $$(3, f(3))$$.
   ____________________________________________________________________________________

#### Exercise 2 (Student Action: Worksheet Example 1)
Given the cubic polynomial $$g(x) = x^3 - 4x$$:
1. Compute the average rate of change on $$[-2, 0]$$.
   $$\text{AROC}_{[-2, 0]} = $$

2. Compute the average rate of change on $$[0, 2]$$.
   $$\text{AROC}_{[0, 2]} = $$

3. **Critical Thinking Prompt:** Notice that $$\text{AROC}_{[-2, 0]} = 0$$ and $$\text{AROC}_{[0, 2]} = 0$$. Does an average rate of change of $$0$$ mean that the function remained constant over these intervals? Explain clearly using the graph or intermediate test points (such as $$x = -1$$ and $$x = 1$$).
   ____________________________________________________________________________________  
   ____________________________________________________________________________________

---

### Objective 2: Determining Concavity via Consecutive Rates

#### Exercise 3 (Tabular Analysis)
The table below records the altitude $$h(t)$$ in meters of a decelerating drone over consecutive 1-second intervals:

| Time $$t$$ (seconds) | Altitude $$h(t)$$ (meters) | Consecutive Interval | $$\Delta t$$ | $$\Delta h$$ | $$\text{AROC} = \frac{\Delta h}{\Delta t}$$ |
| :---: | :---: | :---: | :---: | :---: | :---: |
| $$0$$ | $$10$$ | — | — | — | — |
| $$1$$ | $$25$$ | $$[0, 1]$$ | $$1$$ | $$+15$$ | $$+15\text{ m/s}$$ |
| $$2$$ | $$36$$ | $$[1, 2]$$ | $$1$$ | | |
| $$3$$ | $$43$$ | $$[2, 3]$$ | $$1$$ | | |
| $$4$$ | $$46$$ | $$[3, 4]$$ | $$1$$ | | |

1. Complete the missing cells in the table above.
2. Are the average rates of change increasing, decreasing, or constant?
   ____________________________________________________________________________________
3. State whether $$h(t)$$ is **concave up** or **concave down** on $$(0, 4)$$, and justify your answer using Theorem 1.
   ____________________________________________________________________________________  
   ____________________________________________________________________________________

#### Exercise 4 (Verifying Quadratic Curvature)
Let $$q(x) = ax^2 + bx + c$$, where $$a > 0$$.
1. Choose $$q(x) = x^2$$. Calculate consecutive rates over $$[0, 1]$$, $$[1, 2]$$, $$[2, 3]$$, and $$[3, 4]$$.
    - $$\text{AROC}_{[0, 1]} =$$ _______
    - $$\text{AROC}_{[1, 2]} =$$ _______
    - $$\text{AROC}_{[2, 3]} =$$ _______
    - $$\text{AROC}_{[3, 4]} =$$ _______
2. Formulate a general theorem: For any parabola $$y = ax^2 + bx + c$$, what determines whether the curve is always concave up or always concave down?
   ____________________________________________________________________________________

---

### Objective 3: Points of Inflection & Graphical Geometry

#### Exercise 5 (Connecting Rates to Inflection)
Consider the function $$f(x) = x^3 - 6x^2 + 9x + 2$$.
1. Compute the average rate of change over the following consecutive width-$$1$$ intervals:
    - $$[0, 1]$$: $$f(0) = 2,\; f(1) = 6 \implies \text{AROC} =$$ _______
    - $$[1, 2]$$: $$f(1) = 6,\; f(2) = 4 \implies \text{AROC} =$$ _______
    - $$[2, 3]$$: $$f(2) = 4,\; f(3) = 2 \implies \text{AROC} =$$ _______
    - $$[3, 4]$$: $$f(3) = 2,\; f(4) = 6 \implies \text{AROC} =$$ _______
2. At what $$x$$-value does the rate of change stop decreasing and begin increasing?  
   $$x =$$ _______
3. What special feature occurs on the graph of $$f(x)$$ at this point? State its full coordinates $$(x, y)$$.  
   Point of Inflection: (_____, _____)

#### Exercise 6 (Secant and Tangent Line Geometry)
Check the correct box for each statement based on the geometric properties learned:

| Statement | Concave Up | Concave Down | At Inflection Point |
| :--- | :---: | :---: | :---: |
| The graph of $$f$$ lies entirely **above** its tangent line. | $$\square$$ | $$\square$$ | $$\square$$ |
| The secant chord connecting two points lies **above** the curve. | $$\square$$ | $$\square$$ | $$\square$$ |
| The tangent line **crosses through** the curve. | $$\square$$ | $$\square$$ | $$\square$$ |
| Successive average rates of change over equal intervals are **decreasing**. | $$\square$$ | $$\square$$ | $$\square$$ |

---

## Part 2: Homework Assignment (Independent Practice)

**Pre-AP Algebra 2 — Problem Set 3.4: Rate of Change & Concavity**  
*Show all work neatly. Difference quotients must be explicitly written out before simplifying.*

### Section A: Algebraic Computation of Rates
**1.** For the function $$f(x) = 3x^2 - 5x + 2$$:
- (a) Find the average rate of change on $$[ -1, 2 ]$$.
- (b) Find the average rate of change on $$[ 2, 4 ]$$.
- (c) Find the average rate of change on the arbitrary interval $$[ 1, 1+h ]$$ where $$h \neq 0$$. Simplify completely.

**2.** A hot water thermos cools down after being filled. Its temperature $$T(t)$$ in degrees Celsius after $$t$$ minutes is modeled by the function $$T(t) = \frac{180}{t + 2} + 20$$.
- (a) Calculate the average rate of change in temperature from $$t = 1$$ to $$t = 4$$. Include appropriate units.
- (b) Calculate the average rate of change in temperature from $$t = 4$$ to $$t = 8$$. Include appropriate units.
- (c) Interpret the physical meaning of the sign (positive/negative) of your answers.

---

### Section B: Tabular Analysis & Monotonicity of Rates
**3.** A medical researcher tracks viral cell load counts $$V(t)$$ in thousands of units per milliliter in an in-vitro culture:

| Day $$t$$ | $$0$$ | $$3$$ | $$6$$ | $$9$$ | $$12$$ |
| :--- | :---: | :---: | :---: | :---: | :---: |
| $$V(t)$$ | $$1.2$$ | $$3.6$$ | $$8.4$$ | $$16.8$$ | $$30.0$$ |

- (a) Construct a table showing $$\Delta t$$, $$\Delta V$$, and the average rate of change for each consecutive 3-day interval.
- (b) State whether $$V(t)$$ is concave upward or concave downward on $$[0, 12]$$. Justify using consecutive rates.
- (c) Predict whether the secant line connecting $$(0, 1.2)$$ and $$(12, 30.0)$$ lies **above** or **below** the actual curve between $$t = 0$$ and $$t = 12$$.

**4.** An automotive test car brakes to a halt. The distance $$s(t)$$ in feet traveled after $$t$$ seconds is recorded below:

| $$t$$ (seconds) | $$0$$ | $$1$$ | $$2$$ | $$3$$ | $$4$$ |
| :--- | :---: | :---: | :---: | :---: | :---: |
| $$s(t)$$ (feet) | $$0$$ | $$88$$ | $$150$$ | $$190$$ | $$206$$ |

- (a) Compute the average speed over each 1-second interval.
- (b) What is happening to the vehicle's speed as time progresses?
- (c) Is the position function $$s(t)$$ concave up or concave down? Explain how you know.

---

### Section C: Graphical Interpretation & Points of Inflection
**5.** The graph of a continuous function $$y = p(x)$$ on the domain $$[-5, 5]$$ has the following characteristics:
- $$p'(x) > 0$$ (increasing) on $$(-5, 0)$$ and $$p'(x) < 0$$ (decreasing) on $$(0, 5)$$.
- The average rate of change is **decreasing** on $$(-5, 2)$$.
- The average rate of change is **increasing** on $$(2, 5)$$.
- (a) Identify the open intervals on which $$p(x)$$ is concave up.
- (b) Identify the open intervals on which $$p(x)$$ is concave down.
- (c) At what $$x$$-coordinate does the point of inflection occur? Explain how you determined this.

**6.** **Pre-AP Synthesis & Challenge:**  
Let $$k(x) = x^3 - 3x^2 - 9x + 5$$.
- (a) Find the average rate of change on $$[0, 1]$$ and on $$[1, 2]$$.
- (b) Find the average rate of change on $$[1, 2]$$ and on $$[2, 3]$$.
- (c) The graph of $$k(x)$$ changes concavity at $$x = 1$$. Write a 2–3 sentence explanation verifying whether $$(1, k(1))$$ is a point of inflection by comparing the rates of change to the left and right of $$x = 1$$.

---

## Part 3: Teacher Answer Key & Solutions

### In-Class Worksheet Solutions

#### Exercise 1:
1. $$f(1) = 2(1)^2 - 3(1) + 1 = 0$$  
   $$f(3) = 2(9) - 3(3) + 1 = 10$$  
   $$\text{AROC}_{[1, 3]} = \frac{10 - 0}{3 - 1} = \frac{10}{2} = 5$$
2. $$f(5) = 2(25) - 3(5) + 1 = 36$$  
   $$\text{AROC}_{[3, 5]} = \frac{36 - 10}{5 - 3} = \frac{26}{2} = 13$$
3. Point-slope form through $$(1, 0)$$ with slope $$m = 5$$:  
   $$y - 0 = 5(x - 1) \quad \text{or} \quad y = 5x - 5$$

#### Exercise 2:
1. $$g(-2) = (-2)^3 - 4(-2) = -8 + 8 = 0$$; $$g(0) = 0$$.  
   $$\text{AROC}_{[-2, 0]} = \frac{0 - 0}{0 - (-2)} = 0$$
2. $$g(2) = (2)^3 - 4(2) = 8 - 8 = 0$$; $$g(0) = 0$$.  
   $$\text{AROC}_{[0, 2]} = \frac{0 - 0}{2 - 0} = 0$$
3. **Explanation:** No. An average rate of change of $$0$$ only indicates that the net change between endpoints is zero ($$f(b) = f(a)$$). The function does not remain constant; between $$x = -2$$ and $$x = 0$$, $$g(-1) = 3 > 0$$, so the curve rises to a local maximum before returning to $$0$$.

#### Exercise 3:
1. **Completed Table:**
    - Interval $$[1, 2]$$: $$\Delta t = 1$$, $$\Delta h = 36 - 25 = 11$$, $$\text{AROC} = +11\text{ m/s}$$
    - Interval $$[2, 3]$$: $$\Delta t = 1$$, $$\Delta h = 43 - 36 = 7$$, $$\text{AROC} = +7\text{ m/s}$$
    - Interval $$[3, 4]$$: $$\Delta t = 1$$, $$\Delta h = 46 - 43 = 3$$, $$\text{AROC} = +3\text{ m/s}$$
2. Consecutive rates are strictly **decreasing** ($$15 > 11 > 7 > 3$$).
3. **Concave Down.** By Theorem 1, because the average rate of change over equal subintervals is strictly decreasing, the function $$h(t)$$ is concave down on $$(0, 4)$$.

#### Exercise 4:
1. $$\text{AROC}_{[0, 1]} = \frac{1 - 0}{1} = 1$$  
   $$\text{AROC}_{[1, 2]} = \frac{4 - 1}{1} = 3$$  
   $$\text{AROC}_{[2, 3]} = \frac{9 - 4}{1} = 5$$  
   $$\text{AROC}_{[3, 4]} = \frac{16 - 9}{1} = 7$$
2. **General Theorem:** A quadratic function $$q(x) = ax^2 + bx + c$$ has constant second differences. If $$a > 0$$, successive rates strictly increase ($$\Delta(\text{AROC}) = 2a > 0$$), so the parabola is **always concave up**. If $$a < 0$$, successive rates strictly decrease, so the parabola is **always concave down**.

#### Exercise 5:
1. $$[0, 1]: \frac{6 - 2}{1} = 4$$  
   $$[1, 2]: \frac{4 - 6}{1} = -2$$  
   $$[2, 3]: \frac{2 - 4}{1} = -2$$  
   $$[3, 4]: \frac{6 - 2}{1} = 4$$
2. Rates decrease from $$x = 0$$ to $$x = 2$$ ($$4 \to -2$$), and begin increasing after $$x = 2$$ ($$-2 \to 4$$). The transition occurs at $$x = 2$$.
3. Point of Inflection: $$(2, 4)$$.

#### Exercise 6:
- Graph lies entirely above tangent line $$\to$$ **Concave Up**
- Secant chord lies above curve $$\to$$ **Concave Up**
- Tangent line crosses through curve $$\to$$ **At Inflection Point**
- Successive rates decreasing $$\to$$ **Concave Down**

---

### Homework Problem Set Solutions

**1.** $$f(x) = 3x^2 - 5x + 2$$
- (a) $$f(-1) = 3(1) + 5 + 2 = 10$$; $$f(2) = 3(4) - 10 + 2 = 4$$.  
  $$\text{AROC}_{[-1, 2]} = \frac{4 - 10}{2 - (-1)} = \frac{-6}{3} = -2$$
- (b) $$f(2) = 4$$; $$f(4) = 3(16) - 20 + 2 = 30$$.  
  $$\text{AROC}_{[2, 4]} = \frac{30 - 4}{4 - 2} = \frac{26}{2} = 13$$
- (c) $$f(1) = 3(1) - 5(1) + 2 = 0$$.  
  $$f(1+h) = 3(1+h)^2 - 5(1+h) + 2 = 3(1 + 2h + h^2) - 5 - 5h + 2 = 3h^2 + h$$
  $$\text{AROC} = \frac{f(1+h) - f(1)}{h} = \frac{3h^2 + h - 0}{h} = 3h + 1 \quad (h \neq 0)$$

**2.** $$T(t) = \frac{180}{t + 2} + 20$$
- (a) $$T(1) = \frac{180}{3} + 20 = 80^\circ\text{C}$$; $$T(4) = \frac{180}{6} + 20 = 50^\circ\text{C}$$.  
  $$\text{AROC}_{[1, 4]} = \frac{50 - 80}{4 - 1} = \frac{-30}{3} = -10^\circ\text{C/min}$$
- (b) $$T(8) = \frac{180}{10} + 20 = 38^\circ\text{C}$$.  
  $$\text{AROC}_{[4, 8]} = \frac{38 - 50}{8 - 4} = \frac{-12}{4} = -3^\circ\text{C/min}$$
- (c) The negative sign indicates that the water temperature is **decreasing** over time. Furthermore, the rate changes from $$-10$$ to $$-3$$ (increasing towards zero), meaning the cooling is slowing down (concave up).

**3.** Viral Load Table:
- (a)
    - $$[0, 3]$$: $$\Delta V = 3.6 - 1.2 = 2.4$$; $$\text{AROC} = \frac{2.4}{3} = 0.8\text{ thousand cells/day}$$
    - $$[3, 6]$$: $$\Delta V = 8.4 - 3.6 = 4.8$$; $$\text{AROC} = \frac{4.8}{3} = 1.6\text{ thousand cells/day}$$
    - $$[6, 9]$$: $$\Delta V = 16.8 - 8.4 = 8.4$$; $$\text{AROC} = \frac{8.4}{3} = 2.8\text{ thousand cells/day}$$
    - $$[9, 12]$$: $$\Delta V = 30.0 - 16.8 = 13.2$$; $$\text{AROC} = \frac{13.2}{3} = 4.4\text{ thousand cells/day}$$
- (b) **Concave upward.** The consecutive rates ($$0.8 < 1.6 < 2.8 < 4.4$$) are strictly increasing over equal 3-day intervals.
- (c) Because the function is concave upward, the secant line connecting $$(0, 1.2)$$ and $$(12, 30.0)$$ lies **above** the curve.

**4.** Vehicle Braking:
- (a)
    - $$[0, 1]$$: $$\frac{88 - 0}{1} = 88\text{ ft/s}$$
    - $$[1, 2]$$: $$\frac{150 - 88}{1} = 62\text{ ft/s}$$
    - $$[2, 3]$$: $$\frac{190 - 150}{1} = 40\text{ ft/s}$$
    - $$[3, 4]$$: $$\frac{206 - 190}{1} = 16\text{ ft/s}$$
- (b) The speed is decreasing as time progresses (the vehicle is decelerating).
- (c) **Concave downward.** The rates of change are strictly decreasing ($$88 > 62 > 40 > 16$$).

**5.** Continuous function $$p(x)$$:
- (a) Concave up on $$(2, 5)$$ (where AROC is increasing).
- (b) Concave down on $$(-5, 2)$$ (where AROC is decreasing).
- (c) Point of inflection is at $$x = 2$$, because the concavity changes from concave down to concave up at that coordinate.

**6.** Pre-AP Synthesis:
- (a) $$k(0) = 5$$; $$k(1) = 1 - 3 - 9 + 5 = -6$$; $$k(2) = 8 - 12 - 18 + 5 = -17$$.  
  $$\text{AROC}_{[0, 1]} = \frac{-6 - 5}{1} = -11$$  
  $$\text{AROC}_{[1, 2]} = \frac{-17 - (-6)}{1} = -11$$
- (b) $$k(3) = 27 - 27 - 27 + 5 = -22$$.  
  $$\text{AROC}_{[2, 3]} = \frac{-22 - (-17)}{1} = -5$$
- (c) Notice that for $$x < 1$$, the function's slopes are steepening negatively (rates decreasing), while for $$x > 1$$, the rates rise from $$-11$$ to $$-5$$ (increasing). Because the rate of change transitions from decreasing to increasing at $$x = 1$$, the curvature changes from concave downward to concave upward. Thus, $$(1, -6)$$ is verified as an inflection point.
```eof

### Overview of Generated Materials
- **Part 1: In-Class Guided Worksheet** — Direct companion to your slides. Includes guided difference quotient calculation, the student action problems from the lesson, tabular drone altitude analysis, and a secant/tangent geometry check matrix.
- **Part 2: Homework Assignment (Problem Set 3.4)** — 6 structured problems spanning algebraic difference quotients ($$3h + 1$$), real-world cooling and braking models, viral growth tabular concavity, and higher-order inflection synthesis.
- **Part 3: Complete Teacher Key** — Fully worked algebraic steps, numerical tables, and formal justifications using Theorem 1 and secant/tangent properties.