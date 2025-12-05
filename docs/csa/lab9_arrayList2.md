---
title: Lab ArrayList
nav_order: 15
parent: Labs
layout: page

---
# Lab Array List
{: .no_toc }
## Goals:
{: .no_toc }
* Able to use create and initialize ArrayList
* Able to use basic ArrayList methods

## Task Summary
{: .no_toc .text-delta }
1. TOC
{:toc}

## Task Context
We are building a library application. There are thousands of books in the library. Each book has a ISBN number , title and author information.

Now you are required to complete the following functions to manage books in the library
* manage books information, so that users could look up books by its isbn number, author name and title.
* user could borrow the book if it's available


## Task A: Create Book Class

Moderate
{: .label .label-purple }
Create your own book class to model the books in physical world
* ISBN, title, author are the attributes of the book class.The above information are immutable.
* Create constructors
* Create needed instance methods

Sample code:
```java

public class Book{
    private final String isbn;
    // more attributes
    
    // constructor
    // getter method
}


```

## Task B: Create Library Class
Moderate
{: .label .label-purple }
* Create library class that manages books, such as add new books to the collection, remove books from the collection
* Provide search function to allow user to find books by author or by isbn
* Provide borrow and return service to users. If a book is borrowed already, it won't be available until it's returned

Sample code
```java
public class Library{
    
    private final String libraryName = "My library";
    
   
    public void addBook(Book aBook){
        // your implementation here
    }
    public void removeBook(Book aBook){
        // your implementation here
    }
    
    public ArrayList<Book> searchBookByAuthor(String author){
        // your implementation here
    }
    
    public Book searchBookByISBN(String isbn){
        // your implementation here
    }
    
    public boolean borrowBook(Book book){
        // your implementation here
    }
    
    public boolean returnBook(Book book){
        
    }
    
    public int collectionSize(){
        // your implementation here
    }
    
}


```
