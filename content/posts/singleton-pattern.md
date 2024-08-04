+++
title = "The Singleton Pattern"
date = "2024-01-24"
description = "Article explaining the Singleton Design Pattern by Gang of Four."
+++



Many people argue that Singleton is an anti pattern. That it is a code smell.

{{< figure src="/blog/images/singleton-pattern/singleton-comic.png" caption="A comic depicting the Singleton Pattern (Source: [Refactoring-Guru](https://refactoring.guru/design-patterns/singleton)). Singleton ensures that there is only ONE single instance of an Object. Clients may not even realize that they’re working with the same object all the time." >}}

# Intent



Das Singleton Pattern wird dann verwendet, wenn man von einer Klasse __nur eine einzige__ Instanz erzeugen möchte. Man möchte also verbieten, mehrere Instanzen einer Klasse zu erzeugen.


You have a class and singleton pattern helps you to make it impossible to instantiate that class except for a single time and whenerver you want an instance, you would inevitably have to use that instance. Whenever you ask for an instance, you always get the same instance.


# Main critique points:

1. it is important to avoid globals. Whenever something is globally accessible, it is difficult to control and it might change without you knowing it. Anybody in the whole program could change it. 


2. The assumption: You are assuming that in the future you are only ever ever need a single instance of that particular class. That is not necessarily true. Especially when your applications are growing.



it is completely fine to only have a single object within your application. But its not fine to force, to make it impossible to create a second instance.Because you don't know if you (at some point) will need a second instance.

Das Singleton Pattern wird deshalb von vielen als Code-Smell angesehen und sollte daher eher nicht verwendet werden. "Bad practice!"

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

    // hier kommen noch andere Funktionalitäten hin,
    // wofür die Singleton-Klasse verantwortlich ist.

}
```

Der Konstruktor ist private. D.h. man kann von außerhalb der Klasse keine Instanz erstellen.

Die statische Methode getInstance() kann von außen aufgerufen werden. Beim ersten Aufruf wird eine neue Singleton Instanz erzeugt. Um sicherzustellen, dass immer diese eine Instanz verwendet wird, wird sie in der Variable „instance“ gespeichert. Diese ist statisch.

In der Main-Methode kann man dann folgendes machen:

```java
Singleton s = Singleton.getInstance();
```



{{< figure src="/blog/images/singleton-pattern/singleton2.jpg" caption="" >}}



# Reference

- https://www.youtube.com/watch?v=hUE_j6q0LTQ

- Head First Design Patterns Book

- Book: Design Patterns - Elements of Reusable Object-Oriented Software (1995 Addison-Wesley) by 
Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides

- https://refactoring.guru/design-patterns/singleton

Interesting Talk about Singletons:
https://www.youtube.com/watch?v=-FRm3VPhseI 
