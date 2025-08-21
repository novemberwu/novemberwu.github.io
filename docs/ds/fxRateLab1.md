---
title: FX Rate (OOP) Lab
nav_order: 6
parent: DS Labs
layout: page

---
# FX Rate OOP Lab
{: .no_toc }
## Goals:
{: .no_toc }
* Able to create data type
* Able to define class constructor
* Able to define class methods and instance methods

## Task Summary 
{: .no_toc .text-delta }
1. TOC
{:toc}


## Task A: Design the class 

Design the FX Rate Class, with the following considerations
* What are data members(properties) of the class? 
* What are data types of the properties? 
* Are they mutable or immutable?
  * if mutable, you will have getter and setter methods. Otherwise setter methods should be hidden from client
* What are arguments of the constructor?
* Is your design extendable for more use cases?
  * The data provided is for USD-CNY FX rate? can your design support other currency-pair?
* You might want to  illustrate your ideas with data and examples.
* if facing design choices, you can list out their pros. and cons. and make decisions

{: .important }
>**Deliverable** : a design document that answers the above questions.




## Task B: Implement the class  
Moderate
{: .label .label-purple }
Based on your design document, complete the implementation of the FX rate class

{: .important }
>**Deliverables**
* a ```FxRate``` class that implements your design
* a ```main``` method within the class as a test client
  * within main method, create instances of FxRate
  * print out the **readable** properties of the instances. i.e. fx quote date, rate

