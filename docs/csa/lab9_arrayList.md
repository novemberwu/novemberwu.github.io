---
title: Lab ArrayList Basic
nav_order: 14
parent: Labs
layout: page

---
# Lab Array List
{: .no_toc }
## Goals:
{: .no_toc }
* Able to use create and initialize ArrayList 
* Able to use ArrayList methods



* ![](../../assets/images/ArrayListCheat.png)

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Task A: Explore ArrayList Add , Remove and Size

Easy
{: .label .label-green }

* **TODO #1**: What is the size of ```numbers```
* **TODO #2**: Adding ```System.out.println(numbers.size());``` into the main block then run program
```java
public class ListRunner {
      public static void main(String[] args) {
    
        ArrayList<Integer> numbers = new ArrayList<Integer>();
    
        numbers.add(10);
        numbers.add(20);
        numbers.add(0, 30);
        
      }
}
```

*  **TODO #3**: What is the difference between the add() method with one parameter and the add() method with two parameters?
*  **TODO #4**: After the last add() method, add the line System.out.println(numbers); then run the program
What is printed? What can you conclude about both version of add()
*  **TODO #5**: Add ```numbers.remove(0)``` then run the program. 

## Task B: Explore ArrayList Get/Set
Easy
{: .label .label-green }
* **TODO #1** What is the first number of ```numbers```
* **TODO #2** Adding ```System.out.println(numbers.get(0));``` into the main block then run program
* **TODO #3** Adding ```System.out.println(numbers.get(3));``` into the main block then run program, what will happen?
* **TODO #4** Adding ```numbers.set(0, 999);``` into the main block then run program. then print the arraylist

## Task C: Explore ArrayList indexOf/ Contains

Easy
{: .label .label-green }
* **TODO #1** What is the first number of ```numbers```
* **TODO #2** Adding ```System.out.println(numbers.indexOf(20));``` into the main block then run program
* **TODO #3** Adding ```System.out.println(numbers.contains(3));``` into the main block then run program, what will happen?

## Task D: Explore ArrayList indexOf/Contains on My Data Type

Moderate
{: .label .label-purple }


You are designing your class called Student. You created an ArrayList of students. 
```java
public class Student {
    private final int id;
    private final String name;
    
    public Student(int id, String name){
         this.id = id;
         this.name = name;
    }
    public int getId(){return id;}
    public String getName(){return name;}
    public String toString(){
        return id+"|"+name;
    }
}
```

* **TODO #1** Add the StudentRunner class into your program and run program
What is printed out?



```java
public class StudentRunner {

    public static void main(String[] args) {
        ArrayList<Student> students = new ArrayList<>();

        students.add(new Student(1, "Rachel"));
        students.add(new Student(2, "Monica"));
        System.out.println( "List:" + students);


        Student s = new Student(1, "Rachel");
        System.out.println("List contain "+ s.getName()+"? " + students.contains(s));

    }
}


```

* **TODO #2** adding equals and hashCode  methods to Student class, run StudentRunner again and describe what happened

```java

public class Student {
    private final int id;
    private final String name;

    public Student(int id, String name){
         this.id = id;
         this.name = name;
    }
    public int getId(){return id;}
    public String getName(){return name;}
    @Override
    public String toString(){
        return id+"|"+name;
    }
/* uncomment this code
    @Override
    public boolean equals(Object o){

        if(o == null || !(o instanceof Student)) return false;
        if(this == o) return true;
        Student s = (Student) o;
        return this.id == s.id &&
               (this.name == null ? s.name == null : this.name.equals(s.name));
    }

    @Override
    public int hashCode(){
        int result = Integer.hashCode(id);
        result = 31 * result + (name == null ? 0 : name.hashCode());
        return result;
    }
*/

}




```


