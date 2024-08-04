+++
title = "The Factory Method Pattern"
date = "2024-01-24"
description = "Article explaining the Factory Method Design Pattern by Gang of Four."
+++

# Overview

The Factory Method Design Pattern is oftentimes just called "Factory Pattern". However to be specific, it is called "Factory Method design Pattern" in the original GoF Book.


{{< figure src="/blog/images/factory-method-pattern/factory-method-image.png" caption="Visualization of the Factory Method Pattern (Source: [Refactoring-Guru](https://refactoring.guru/design-patterns/factory-method))" >}}

# Intent
_Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses._

Simply put:
In the end, i want to have an object created. However I don't necessarily know how I want to create that object or why I want to create that object or which parameter I have to pass when constructing that object. These are all unknowns and thats why I want to defer the instantiation process to a Factory/Factories.

# Pattern-Type
Das Factory-Method Pattern fällt unter die Kategorie "Creational Pattern", weil dieses Pattern eine Möglichkeit bietet, Objekte zu erstellen. Mithilfe dieses Patterns kann man Objekte erstellen, ohne dass man die Erstellungslogik der Objekte preis gibt (herzeigt).

# Structure

Structure described (similar) as in the original GoF Design Patterns Book by Erich Gamma et. al (S. 108)

{{< figure src="/blog/images/factory-method-pattern/factory-method.png" caption="" >}}


* __Product__: Defines the interface of objects the factory method creates.
* __Concrete Product__: A class that implements the Product Interface.
* __Factory__: Defines the interface of factories, that create Products. It declares the factory method, which returns an object of type product.
* __Concrete Factory__: A class that implements/overrides the factory method to return an instance of a Concrete Product.

Note: Statt den Interfaces Product und Factory können auch abstrakte Klassen oder "normale" Klassen verwendet werden, von denen dann jeweils die anderen Klassen erben. Es kommt immer auf die jeweilige Situation an, was besser passt.


# The Problem

Why do we need this pattern? Why do we need such a thing as a factory?

In OOP verwendet man überall im Code viele Objekte. Irgendwo müssen diese Objekte auch instanziiert werden.

Die Konstruktion eines Objekts wird ausgelagert (in eine Factory).

Factory method pattern says: When you are about to instantiate a new Object, lets encapsulate that instantiation so that we can make it uniform across all places. You can use the factory whenever you want to instantiate and the factory is responsible for instantiating approperately. Es ist also eine Art "Wrapper" rundum das `new` keyword. Why do you want to create such a wrapper?

1. It could be possible, that the instantiation process is very complex. For instance it could be that you need a lot of business logic to determine which parameters you have to pass to the constructor of the Objects you want to create. Or you need a lot of business logic to determine which type of objects you actually want to construct. Then the Factory Method pattern comes in handy because you can encapsulate that instantiation process. Because what if you would need the same instantiation process multiple times in different parts of your code? Then you could use the same Factory at all that different parts to construct that object. In this way you avoid code duplication (copy and pasting code).

2. Polymorphism: If you have a Factory that wraps your costruction and if that factory is an instance, then you can swap that instance (at runtime) for an instance of another Factory (Vorrausgesetzt die beiden Factories implementieren natürlich dasselbe Interface).

3. Example: You have a Dog, Cat and Duck class which all implement the IAnimal interface. In the main method, you want to instantiate one of these three concrete classes. However when the program enters the main method, it does not know which one of these three classes it wants to instantiate. Maybe you want to randomize which one of these classes have to be instantiated. 

The Factory is responsible for holding "the business Logic of creation" of something of a particular shared type (for instance IAnimal). It is responsible for a particular creation mechanism of Animals. 

# An example for the Pattern


{{< figure src="/blog/images/factory-method-pattern/factory-method-detail.jpg" caption="" >}}




... Hier das Beispiel auf dem Bild erklären ...




Anstatt
```java
Shape shape1 = new Circle();
```

kann man
```java
Shape shape1 = shapeFactory.getShape("circle");
```
machen.

Wie die Objekte erstellt werden, also die Logik, kann somit versteckt werden.

Zum gezeichneten Beispiel:

BlueShapeFactory und RedShapeFactory sind zwei versschiedene ShapeFactories, die Shapes erzeugen. Sie erzeugen die Shapes auf unterschiedliche Weise.
In der Main-Methode könnte dann z.b. soetwas stehen:
```java
Shape shape = redShapeFactory.getShape("Square");
shape.draw();
```

Man kann also unterschiedliche und beliebig viele Factories erstellen, die Objekte auf unterschiedliche Weise erstellen.

Bei dem Factory Method Pattern erzeugt jede konkrete Factory jeweils nur ein Objekt. Das ist der Unterschied zum Abstract-Factory-Pattern. Beim Abstract-Factory-Pattern erzeugt jede konkrete Factory __mehrere__ unterschiedliche Objekte.

# Reference

- https://www.tutorialspoint.com/design_pattern/factory_pattern.htm

- https://www.youtube.com/watch?v=EcFVTgRHJLM

- Book: Design Patterns - Elements of Reusable Object-Oriented Software (1995 Addison-Wesley) by 
Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides

- https://refactoring.guru/design-patterns/factory-method

- University Lecture called “Software Design Methods” by
Ing. Dr. Hans J. Prüller, B.Sc.
Personal website: https://hansprueller.lbs-logics.com/ 

- Head First Design Patterns Book