---
title: Lab Class Static Variables/ Methods
nav_order: 19
parent: Labs
layout: page

---
# Lab Class Static Variables/ Methods
{: .no_toc }
## Goals:
{: .no_toc }
* Understand static variables and static methods


## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}


## Task A
Easy
{: .label .label-green }

* ✅ TO DO #1: Look at the numMembers variable in the ArtClubMember class and the ArtClubMember constructor.
What do you notice about the way it is declared and initialized? How is it used in the constructor?
Add the following code to ClubRunner:```System.out.println(ArtClubMember.numMembers);```
What happens when you run the program? Why do you think this happened?

* ✅ TO DO #2: Change the code you added to System.out.println(eva.numMembers);
Do you get the same result? Why or why not?


```java
/*
 * Represents a member of an art club
 */
public class ArtClubMember {

    private String name;                 // The name of the club member
    public static int numMembers = 0;   // The number of club members

    /*
     * Sets name to the specified name
     */
    public ArtClubMember(String name) {
        this.name = name;
        numMembers++;
    }

    /*
     * Returns the name of the club member
     */
    public String getName() {
        return name;
    }

    /*
     * Returns the number of club members
     */
    public static String getNumMembers() {
        return "The Art Club has " + numMembers + " members.";
    }

}
```
```java

public class ClubRunner {
    public static void main(String[] args) {

        ArtClubMember eva = new ArtClubMember("Eva");
        ArtClubMember jacob = new ArtClubMember("Jacob");
        ArtClubMember anita = new ArtClubMember("Anita");

        System.out.println(ArtClubMember.getNumMembers());

        /* ---- 🔎 ADD YOUR CODE BELOW THIS LINE ---- */





    }
}
```
## Task B
Experiment with the program by making the following modifications, then run the program to observe the results.
* ✅ TO DO #1: In ArtClubMember.java, look at the getNumMembers() method. What do you notice about the method signature?
Remove the static keyword from the getNumMembers() method signature. What happens when you run the program? Why do you think this happened?
* ✅ TO DO #2: Add the static keyword back to the getNumMembers() method signature. In ClubRunner.java, try calling the getNumMembers() method on one of the ArtClubMember objects. What happens when you run the program?

## Task C
Experiment with the program by making the following modifications, then run the program to observe the results.
* ✅ TO DO: The ArtClubMember class has a static variable numMembers that is used in the getNumMembers method.
Remove the static keyword from the static variable. What happens when you run the program? Why do you think this happened?