+++
title = "Polymorphism and dynamic binding"
date = "2024-10-05"
description = "This article explains the concepts of polymorphism and dynamic binding in the Java programming language in detail."
+++

{{< figure src="/images/polymorphism/chameleon.jpg" caption="By changing its color, a chameleon can take on different states – similar to an object in programming (Source: [Pexels](https://www.pexels.com/photo/green-chameleon-in-nature-19730407/))." width="50%">}}

## Overview

Polymorphism, along with encapsulation and inheritance, is a fundamental concept of object-oriented programming. But what exactly is polymorphism? This blog article aims to answer that question. Essentially, there are two types of polymorphism – static and dynamic polymorphism. 

## Static Polymorphism (Compile-Time Polymorphism)

This is normal method overloading. When you have a class with two methods that have the same name but different parameter lists, this is called "Compile-Time Polymorphism" or static polymorphism. Here, the compiler determines which of the methods will be called based on the number and types of parameters provided.

## Dynamic Polymorphism (Run-Time Polymorphism)

In dynamic polymorphism, another mechanism comes into play – method overriding. But let's start with an example.

In OOP, every object of a derived class is also an object of its superclass. This means: You can assign subclass objects to superclass object variables --> thus, the object becomes "polymorphic" (= manifold). Why is this possible? Because the superclass and the subclass share the same interface. The same interface means, in this context, the same method signatures and public properties.

The following code example shows a polymorphic object:

```java
Animal myAnimal = new Dog();
```

In this line, an object of the class `Dog` is created and stored in an object variable of type `Animal`. This is possible because the class Dog inherits from the class Animal. Accordingly, a Dog **IS** also an Animal, and the variable of type Animal can contain an object of type Dog. This is called polymorphism because the reference of the superclass type (Animal) points to an object of the subclass (Dog). Since `myAnimal` can be either a Dog object or a Cat object at runtime, it is polymorphic, meaning it has multiple forms.

The following image illustrates the entire concept of dynamic polymorphism in a single picture created by me:


{{< figure src="/images/polymorphism/polymorphism.jpg" caption="Polymorphism at a glance.">}}

## Dynamic binding

What does _"Dynamic Binding"_ mean? In the previously mentioned example, the decision of which method (`write()` of the superclass or `write()` of the subclass) to call is made at runtime. The JVM determines during runtime, based on the object type, which method to use. This is called dynamic binding.

## Type conversion

Similar to primitive data types, implicit and explicit type conversion also works with objects. The class "Object" is used as an example here. Every object in Java automatically inherits from the class `java.lang.Object`.

### Implicit type conversion

```java
// Mountainbike inherits from Object
Mountainbike m = new Mountainbike();
Object obj = m;

// primitive datatypes
int i = 5;
double d = i;
```

### Explicit type conversion

```java
// Mountainbike inherits from Object
Object obj = new Object();
Mountainbike bike = (Mountainbike) obj;

// primitive datatypes
double d = 7.321;
int i = (int) d;
```

## Upcasting and Downcasting

Let’s assume the class `Dog` inherits from the class `Animal`.

The following code describes "upcasting." This works automatically in Java. An object is taken and "casted up" to its superclass.

```java
Animal animal = new Dog();
```

The following code throws an error because you cannot directly assign an `Animal` reference to a `Dog` reference without casting it down first.

```java
Dog myDog = animal; // ERROR!
```

The following code describes "downcasting." An object is taken and "casted down" to the subclass.

```java
Dog myDog = (Dog) animal;
```

It is advisable to use the `instanceof` operator before downcasting to ensure that the `Animal` is actually a `Dog`, in order to avoid a `ClassCastException` at runtime:

```java
if (animal instanceof Dog) {
    Dog myDog = (Dog) animal; // Safe Downcasting
}
```

Some people find that downcasting is a code smell. For example, Christopher Okhravi, who made an interesting [video](https://www.youtube.com/watch?v=ECTyHSZUv0c) on this topic, argues that downcasting could violate contracts in the code and also violates the "Open-Closed Principle".

## Polymorphism via interfaces

The application of polymorphism through interfaces is probably the most well-known method. When an interface is used as the data type of an object variable and an object that implements the interface is assigned to it, polymorphism is utilized. Example:

```java
interface Moveable {
    void move();
}

class Car implements Moveable {
    public void move() {
        System.out.println("The car is driving.");
    }
}

class Robot implements Moveable {
    public void move() {
        System.out.println("The robot is walking.");
    }
}

public class Main {
    public static void main(String[] args) {

        Moveable moveableObject;

        moveableObject = new Car();
        moveableObject.move();  // The car is driving.

        moveableObject = new Robot();
        moveableObject.move();  // The robot is walking.
    }
}
```

This is the great power of polymorphism --> objects sharing the same interface can be easily exchanged (at runtime). Plug and Play! See: "Dependency Inversion Principle".

For example, you can now pass both a ``Car`` and a ``Robot`` object to the following method:

```java
void startMoving(Moveable moveableObject) {
        moveableObject.move();
}
```

## The pillars of OOP?

You hear it everywhere - the three pillars of object-oriented programming are encapsulation, inheritance, and polymorphism. I don't know who originally defined it this way, but this statement is on everyone's lips. Even during my studies, professors repeatedly said this, and in the software engineering community, this has now become a standard statement that many people make without really thinking about it. Often, when asked, _"What is OOP anyway?"_, the answer is _"OOP consists of encapsulation, inheritance, and polymorphism."_.

Recently, I read the book "Clean Architecture" by Robert C. Martin. In Chapter 5 of the book (starting on p. 33), Martin describes that the concepts of encapsulation, inheritance and polymorphism existed long before the development of object-oriented languages. He mentions, for example, that encapsulation was already perfectly implemented in the programming language C. In languages like Java or C#, encapsulation is only possible in a weakened form because it is impossible to separate the declaration and definition of a class.

According to him, inheritance could also be practiced in C already (albeit with a specific trick). He notes that while object-oriented languages made inheritance simpler and more pleasant for programmers, the concept was already there before. The book also describes that polymorphism could already be practiced in C and was not something new in object-oriented languages. However, the application of polymorphism in OO languages became much easier.

> Using an OO language makes polymorphism trivial. That fact provides an enormous power that old C programmers could only dream of. On this basis, we can conclude that OO imposes discipline on indirect transfer of control.<br>
> — <cite>Robert C. Martin | Clean Architecture S. 42</cite>

Martin describes polymorphism as particularly powerful because it decouples dependencies between components and allows systems to be designed in a more modular and easily extendable way. The keyword here is "Plugin Architecture."

> OO is the ability, through the use of polymorphism, to gain absolute control over every source code dependency in the system. It allows the architect to create a plugin architecture, in which modules that contain high-level policies are independent of modules that contain low-level details. The low-level details are relegated to plugin modules that can be deployed and developed independently from the modules that contain high-level policies.<br>
> — <cite>Robert C. Martin | Clean Architecture S. 47</cite>

From this quote, I conclude that the ability to apply the Dependency Inversion Principle (DIP) in a straightforward way is the fundamental characteristic of object-oriented programming. This principle is based on the use of polymorphism. Without polymorphism, there is no DIP. Therefore, I would personally say that the most important foundational characteristic of OOP is polymorphism.

## Reference

### Books

- Robert C. Martin: _Clean Architecture: A Craftsman's Guide to Software Structure and Design (Robert C. Martin Series)_, Publisher: Pearson, Year: 2017, ISBN: 0134494164

### Videos

- Video from Sriyank Siddhartha: _Java Polymorphism: Compile time vs. Run time. Method Overloading vs. Overriding_, URL: https://www.youtube.com/watch?v=W2pTtlNaa6s, Last access: 05.10.2024.

- Video from "CodingWithJohn": _Upcasting and Downcasting in Java - Full Tutorial_, URL: https://www.youtube.com/watch?v=HpuH7n9VOYk, Last access: 05.10.2024.

- Video from Christopher Okhravi: _Downcasting Is A Code Smell | Code Walks 043_, URL: https://www.youtube.com/watch?v=ECTyHSZUv0c, Last access: 05.10.2024.

### AI

- ChatGPt