---
title: FX Rate (Array) Lab
nav_order: 7
parent: DS Labs
layout: page

---
# FX Rate Array Lab
{: .no_toc }
## Goals:
{: .no_toc }
* Able to create arrays
* Able to use array to store data
* Able to write code to traverse the array

## Task Summary 
{: .no_toc .text-delta }
1. TOC
{:toc}


## Task A: Create array to store FX rate objects 
Moderate
{: .label .label-purple }

Write a program to read USD-CNY FX rate historical data file, and store them in an array

- Modify your last lab [FX Rate Lab] code, to store the FX rate in an array 
  - You need to first create an array with fixed size 30
  - For each line of the file, create an instance of FX Rate
  - Store the instance to the array
- Verify your code by iterating each element, and print out

{: .note }
You might want to overload FX rate class ```toString()``` method, to make print easier

Sample input
```
java fxRateArray
```
Sample output
```
08/07/2025,7.1804
08/06/2025,7.1826
08/05/2025,7.1840
...
```

{: .note }
>Please use the following code to  read line by line

```java
// define the scanner to source the input from the file
// create an array of FX rates, with fixed size 30
    while( scanner.hasNext()){ // test if program read to the end of the file
            String text = scanner.next(); // get the next line from file
            // TASK A: Your code here.
            // Instead of printing out, new FX rate objects and store them in the array
    }
}
```


## Task B: Calculate the average FX rate
Moderate
{: .label .label-purple }
Since you already get the fx rates arrays, write a program to iterate the array and calculate the average fx rate

{: .note }
The order of the output is reversed order of original file

Sample input
```
java fxRateAverage
```
Sample output
```
7.1781625
```

## Task C: Search FX rate of a given date

Moderate
{: .label .label-purple }
Since you already get the fx rates arrays, write a program search the fx array for a given date, return null if not found

{: .note }
The order of the output is reversed order of original file

Sample input
```
java fxRateSearch 08/06/2025
```
Sample output
```
08/06/2025,7.1826

```



[FX Rate Lab]: {% link docs/ds/fxRateLab.md %}


