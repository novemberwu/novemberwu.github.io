---
title: FX Rate Lab
nav_order: 5
parent: DS Labs
layout: page

---
# FX Rate Lab
{: .no_toc }
## Goals:
{: .no_toc }
* Able to call static methods of Math API
* Able to implement static method 
* Aware of the mechanism of passing arguments to functions 
* Able to use condition and loops to process data

## Task Summary 
{: .no_toc .text-delta }
1. TOC
{:toc}



## Task A: Read FX rate from a file
Moderate
{: .label .label-purple }

Write a program to read USD-CNY (US dollar to Chinese Yuan) FX rate historical data file
And print out the content to the standard output

* Copy the [USD_CNY_FX.csv] file to project directory of your java program
* Use ```Scanner``` to read file content line by line
* Use ```System.out.println``` to print the content

Sample input
```
java fxRate
```
Sample output
```
08/07/2025,7.1804
08/06/2025,7.1826
08/05/2025,7.1840
...
```

{: .note }
>Please use the following code to read line by line

```java
// define the scanner to source the input from the file
    while( scanner.hasNext()){ // test if program read to the end of the file
        String text = scanner.next(); // get the next line from file
        // TASK A: Your code here, process one line of text
    }
}
```

## Task B: Parse numbers from the file
Hard
{: .label .label-red }

Write a method that produces the USD-CNY fx rate when given a date, and write a main function to call it and test it
* if the rate of that day is given in the file, output the rate
* otherwise, output "unknown"
* for now you can assume date format from program input and fx file are the same (i.e. no format change is required)


Sample input
```
java FxRate 08/06/2025
java FxRate 08/02/2025
```
Sample output
```
7.1826
null
```
Your method signature should be 
```java
public Double getFxRate(String date){
        // TASK B: your code is here
        return null; // if rate of a given date is not found, return null
}
```

{: .note }
> * You may need to use String.split to split one line of text, into two parts, i.e. date and rate
> 
> * Compare the date String from the file and that from program input, you will get the rate of that date
> 
> * You may use String.trim to remove leading and trailing space to make your program tolerant to extra space from input
> 
> * You may use Double.parseDouble to parse text into double
> 
> 





[USD_CNY_FX.csv]: {% link docs/csa/data/USD_CNY_FX.csv %}


