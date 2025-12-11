---
title: Lab ArrayList Algorithms
nav_order: 17
parent: Labs
layout: page

---
# Lab Array List Algorithms
{: .no_toc }
## Goals:
{: .no_toc }
* Able to implement the ArrayList algorithms


## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}


## Task A: Calculate average age of players whose age is under 18

Easy
{: .label .label-green }

The ```GameAccount``` class is  used to store information about players of an online game. 
A partial declaration of the ```GameAccount``` class is shown

```java

public class GameAccount
{
    //return the username of the player
    public String getUsername()
    {/*implementation is not shown*/}
    
    // return the age of the player
    public int getAge()
    {/*implementation is not shown*/}
    
    /*other instance variables , constructors and methods that are not shown*/
}
```

The ```GamePlatform``` class maintains an ArrayList of ```GameAccount``` objects named allPlayers.
A partial declaration of the ```GamePlatform``` class is shown

```java
public class GamePlatform
{
    public ArrayList<GameAccount> allPlayers;

    /**
     * return the average age of players in allPlayers whose age is less than 18
     * @return the average age
     * Preconditions: allPlayers contains at least one player whose age is under 18
     * allPlayers is not null and contains no null elements
     */
    public double avgTeenAccounts(){
        // TASK A: your implementation here
    }
    
    /**
     * remove the teen players whose age is less than 18, from allPlayers
     * @return the count of the removed accounts
     * Preconditions: allPlayers contains at least one player whose age is under 18
     * allPlayers is not null and contains no null elements
     */
    public int removeTeenAccounts(){
        // TASK B: your implementation here
        
    }
    
}
```
Write the GamePlatform method avgTeenAccounts. The method should return the average age of the players in allPlayers whose age is under 18
```java
/**
     * return the average age of players in allPlayers whose age is less than 18
     * @return the average age
     * Preconditions: allPlayers contains at least one player whose age is under 18
     * allPlayers is not null and contains no null elements
     */
    public double avgTeenAccounts(){
        // TASK A: your implementation here
    }
```

## Task B: Remove players from allPlayers whose age is under 18
Moderate
{: .label .label-purple }

Write the GamePlatform method removeTeenAccounts. The method should remove all teen players from allPlayers
```java
 /**
     * remove the teen players whose age is less than 18, from allPlayers
     * @return the count of the removed accounts
     * Preconditions: allPlayers contains at least one player whose age is under 18
     * allPlayers is not null and contains no null elements
     */
    public int removeTeenAccounts(){
        // TASK B: your implementation here
        
    }
```