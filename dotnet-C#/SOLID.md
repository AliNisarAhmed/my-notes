# SOLID 

## Single Responsibility Principle 

Single Responsibility Principle or [SRP](http://en.wikipedia.org/wiki/Single_responsibility_principle) states that **every class should have a single responsibility**. There should **never be more than one reason for a class to change**.

## Open Closed Principle 


Open/Closed Principle or [OCP](http://en.wikipedia.org/wiki/Open/closed_principle) states that software entities should be **open for extension**, but **closed for modification**.

Modification means changing the code of an existing class, and extension means adding new functionality.

But how are we going to add new functionality without touching the class, you may ask. It is usually done with the help of interfaces and abstract classes.

You should make all member variables **private** by default. Write getters and setters only when you need them.

## Liskov Substitution Principle 

Liskov Substitution Principle or [LSP](http://c2.com/cgi/wiki?LiskovSubstitutionPrinciple) states that objects in a program should be **replaceable with instances of their subtypes without altering the correctness** of the program.

The Liskov Substitution Principle states that subclasses should be substitutable for their base classes.

This means that, given that class B is a subclass of class A, we should be able to pass an object of class B to any method that expects an object of class A and the method should not give any weird output in that case.

## Interface Segregation Principle 

The principle states that many client-specific interfaces are better than one general-purpose interface. Clients should not be forced to implement a function they do no need.

Interface Segregation Principle or [ISP](http://en.wikipedia.org/wiki/Interface_segregation_principle) states that **many** client-specific **interfaces are better than one** general-purpose interface. In other words, you should not have to implement methods that you don’t use. Enforcing ISP gives you **low coupling**, and **high cohesion**.

## Dependency Inversion Principle 

The Dependency Inversion principle states that our classes should depend upon interfaces or abstract classes instead of concrete classes and functions.