+++
title = "The Singleton Pattern - A code smell?"
date = "2024-01-24"
description = "Article explaining the Singleton Design Pattern by Gang of Four."
+++

This blog post offers an explanation of Singleton Design Pattern by Gang of Four. The examples are written in Java.

{{< figure src="/images/singleton-pattern/singleton-comic.png" caption="A comic depicting the Singleton Pattern (Source: [Refactoring-Guru](https://refactoring.guru/design-patterns/singleton)). Singleton ensures that there is only ONE single instance of an object. Clients may not even realize that they’re working with the same object all the time." >}}

# Intent

The original GoF Design Pattern book defines the Singleton Pattern as follows: **Ensure a class only has one instance, and provide a global point of access to it.**


In other words:

The Singleton pattern is used when you want to create __only a single instance__ of a class. The goal is to prevent the creation of multiple instances of the same class.

You have a class and singleton pattern helps you to make it impossible to instantiate that class except for a single time and whenever you want an instance, you would inevitably have to use that instance. Whenever you ask for an instance, you always get the same instance.

Many people argue that Singleton is an "anti pattern" or "code smell". But why?

# Main critique points:

1. Singletons are globals and it is important to avoid globals in software engineering. Because whenever something is globally accessible, it is difficult to control and it might change without you knowing it. Anybody in the whole program could change it. 

2. "The wrong assumption": You are assuming that in the future you are only ever ever need a single instance of that particular class. That is not necessarily true. Especially when your applications are growing.

It is completely fine to only have a single object of a class within your application. But its not fine to make it impossible to create a second instance. Because you don't know if you (at some point) will need a second instance.

There is an interesting [Google TechTalk](https://www.youtube.com/watch?v=-FRm3VPhseI) by Miško Hevery (who is the creator of the AngularJS framework btw.) about Singletons and their global state. In his talk, he describes that Singletons are really a nightmare and justifies his opinion with code examples. To sum up the talk:
- Singletons make your code untestable.
- Each Singleton generates an infinite number of additional global variables because everything that is accessible to a global variable is global as well.

I also agree with these opinions and consider the singleton pattern more as a code smell than a pattern that should actually be used. However, let's take a look at the pattern in detail:

# Pattern

```java

class Singleton{

    private static Singleton instance;

    private Singleton(){ ... }

    public static Singleton getInstance(){
        
        if(this.instance == null){
            this.instance = new Singleton();
        }

        return this.instance;
    }

    // Additional functionalities will be added here,
    // for which the Singleton class is responsible.

}
```

The constructor is private, meaning that no instance can be created from outside the class.

The static method `getInstance()` can be called externally. On the first call, it creates a new Singleton instance. To ensure that this single instance is always used, it is stored in the static variable `instance`.

In the `main` method, you can then do the following:

```java
Singleton s = Singleton.getInstance();
```
 
# Reference

## Video
- Video from Christopher Okhravi: _Singleton Pattern – Design Patterns (ep 6)_, URL: https://www.youtube.com/watch?v=hUE_j6q0LTQ, Last Access: 14th October 2024.

## Books
- Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides: _Design Patterns – Elements of Reusable Object-Oriented Software_, Publisher: Addison-Wesley, Year: 1995, ISBN: 0201633612

- Eric Freeman, Elisabeth Robson, Kathy Sierra and Bert Bates: _Head First Design Patterns: Building Extensible and Maintainable Object-Oriented Software 2nd Edition_, Publisher: O'REILLY, Year: 2021, ISBN: 9781492078005

## Website
- Refactoring Guru Website: _Singleton_, URL: https://refactoring.guru/design-patterns/singleton, Last Access: 14th October 2024.

## Talk

- Google TechTalk by Miško Hevery: _The Clean Code Talks: Global State and Singletons_, Talk-Date: November 13, 2008, URL: https://www.youtube.com/watch?v=-FRm3VPhseI, Last Access: 14th October 2024.
 
