+++
title = "Das Strategy Pattern"
date = "2024-09-12"
description = "Artikel, der das GoF Strategy Design Pattern erklärt."
+++

Dieser Blog Artikel erklärt das GoF Strategy Design Pattern. Die Beispiele wurden in Java verfasst.

{{< figure src="/images/strategy-pattern/duck.jpg" caption="Eine Ente kann fliegen, quacken (und wahrscheinlich kann sie noch viele weitere Sachen). [Image-Source: Pixabay](https://pixabay.com/photos/duck-mallard-bird-pond-plumage-8510483/)" >}}


## Welches Problem löst das Strategy Pattern?

Angenommen, wir haben eine `Duck`-Klasse. Und dann haben wir eine `CityDuck` (die in der Stadt lebt) und eine `WildDuck` (die im Wald lebt). Wir verwenden Vererbung – also CityDuck und WildDuck erben von Duck. Die Unterklassen sind dafür verantwortlich, ihre eigene Version der `display()`-Methode zu implementieren. Wildenten können sich zum Beispiel anders anzeigen lassen als Stadtenten. Sie müssen also die `display()`-Methode überschreiben. Die `quack()`-Methode wird jedoch von allen diesen Unterklassen gemeinsam genutzt. Hier wird die Vererbung also zur Codewiederverwendung genutzt.

{{< figure src="/images/strategy-pattern/1.svg" caption="`CityDuck` und `WildDuck` erben von `Duck`" >}}

Was ist das Problem hier? Das Problem ist **VERÄNDERUNG** (die ständig passiert). Die einzige Konstante in der Softwareentwicklung ist "Veränderung"! Wenn sich Anforderungen ändern, ist unser aktuelles Softwaredesign möglicherweise nicht mehr für die neuen Anforderungen geeignet. Ein Beispiel für eine solche Änderung könnte das folgende sein:

Was ist, wenn wir eine weitere Methode hinzufügen, zum Beispiel `fly()`? Denn Enten können fliegen. Wir fügen die Methode in der Elternklasse `Duck` hinzu. Aber dann möchten wir eine neue Klasse namens `RubberDuck` einführen, die von `Duck` erbt. `RubberDuck` hat ihre eigene `display()`-Methode. Aber Gummienten können nicht fliegen. Jetzt haben wir ein Problem! Gummienten können nicht fliegen, erben jedoch die `fly()`-Methode von ihrer Elternklasse `Duck` –> das ist falsch! ⚠️

{{< figure src="/images/strategy-pattern/2.svg" caption="`RubberDuck` erbt die `fly()`-Methode! Jetzt kann jeder, der ein `RubberDuck`-Objekt verwendet, die `fly()`-Methode auf diesem Objekt aufrufen, was nicht möglich sein sollte, da Gummienten ja nicht fliegen können.⚠️" >}}

And what if we wanna add a new class called `MountainDuck` (which lives in the mountains). This duck implements its own `display()` method. But it implements also its own `fly()` method because the duck has a specific way to fly (ultrafast for instance).
And then we add a new class called `CloudDuck`. This duck also overrides `display()` and `fly()` methods. But the cloud-duck flys EXACTLY the same as the mountain-duck. What are we doing now? We cannot reuse the same code from the mountain-duck. So we have to copy and paste. And that is very, very bad! ⚠️ (cf. DRY Principle)

{{< figure src="/images/strategy-pattern/3.svg" caption="" >}}

Some might say you could solve some of these problems with "more inheritance". You could for instance introduce a new base class just for `MountainDuck` and `CloudDuck` where the both ducks are inheriting from. But the more classes you create and inherit from, the more "inflexible" all of that gets. 

The problem is, that you can't share behaviour over classes that are in the same hierarchy (horizontally). Inheritance only works by sharing code from top to bottom.
{{< figure src="/images/strategy-pattern/4.svg" caption="When it comes to inheritance, code cannot be shared between classes which are in the same hierarchy." >}}

The solution to problems with inheritance is not "more inheritance". The solution is **composition**! And that's exactly what the strategy pattern does.

## Overview
Warum ist das Strategy Pattern so nützlich? Wegen der Flexibilität! Bei diesem Pattern werden Verhaltensweisen aus einer Klasse (Context) in andere Klassen (Concrete Strategies) ausgelagert und via Interfaces lose an die Klasse (Context) gekoppelt. Die Kontext Klasse ruft dann lediglich den Code der konkreten Strategies auf. Dadurch können Objekte und deren Verhaltensweisen komplett easy zusammengebaut werden. Hier ein Beispiel, wie ein Client das Strategy Pattern verwenden könnte:

```java
public static void main(String[] args) 
{
	Duck duck1 = new Duck(new LowFlying(), new LoudQuacking());
	duck1.fly(); // prints "Low Flying"
	duck1.quack(); // prints "Loud Quacking"

	Duck duck2 = new Duck(new HighFlying(), new QuietQuacking());
	duck2.fly(); // prints "High Flying"
	duck2.quack(); // prints "Quiet Quacking"
}
```

Die Verhaltensweisen werden den Enten via Dependency Injection übergeben.

{{< figure src="/images/strategy-pattern/strategy.jpg" caption="Objekte werden mit Verhaltensweisen zusammengebaut (lose gekoppelt)." >}}

Das Strategy-Pattern ist ein Paradebeispiel dafür, dass Composition besser ist als Inheritance. We need Inheritance much much less than we believe.

The concrete implementations (LoudQuacking, QuietQuacking, etc.) can vary just as much as they want – without having to change the code of the context class (Duck). And that is the power of the strategy pattern!


## The Pattern itself
This is how the full pattern looks like in UML (Unified Modeling Language):

{{< figure src="/images/strategy-pattern/5.svg" caption="UML Diagram of the Strategy Pattern." >}}

The `Context` class holds one or more objects of type Strategy as member variable. The member methods of the `Context` class then simply call the `behaviour()` methods implemented in the `ConcreteStrategy` classes.

In the following, the full duck example is displayed in UML. In contrast to the UML above, the duck example shows how the pattern works with multiple Strategies (`IFlyBehaviour` and `IQuackBehaviour`):

{{< figure src="/images/strategy-pattern/6.svg" caption="One example of the strategy pattern in action. " >}}

## Intent of the strategy pattern

The original GoF Design-Pattern Book states the intent of the strategy pattern as follows:  
**_"Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it."_**

In the following, I will try to explain each phrase of the intent individually. Note! In this original intent the term  _"family of algorithms"_ means the same as "strategy". In the following, the word strategy is mainly used instead of _"family of algorithms"_.

**_"Family of algorithms"_:**  
in diesem Beispiel können die Klassen IFlyBehavior und IQuackBehavior als "Familien" angesehen werden.

**_"Algorithms"_:**  
Sind in diesem Fall die Verhaltensweisen (= konkreten implementierungen der Strategy). Das sind LowFlying, Highflying, LoudQuacking und QuietQuacking. LowFlying und Highflying gehören zur Familie IFlyBehavior. LoudQuacking und QuietQuacking gehören zur Familie IQuackBehavior.

**_"Strategy lets the algorithm vary independently from clients that use it"_:**    
Jede "Concrete Strategy Class" hat ihr eigenes, unabhängiges Verhalten. Diese konkreten implementierungen der Familien IFlyBehavior und IQuackBehavior können komplett unterschiedlich zueinander sein.

**_"make them interchangeable"_:**  
Konkrete Behaviors können ganz leicht ausgetauscht werden (solange sie dasselbe Interface implementieren).
z.B. kann man eine neue Klasse "FastFlying" einführen, die "IFlyBehavior" implementiert. Und schon kann man der `duck1` das Verhalten `new FastFlying()` anstatt dem Verhalten `new LowFlying()` übergeben. Here, the algorithm gets decoupled from the one that is using the algorithm. Whoever is using the algorithm is not forced to change when one of the algorithms is changed.

## When should you use this pattern?

- Use the Strategy pattern when you want to use different variants of an algorithm within an object and be able to switch from one algorithm to another during runtime.

- Use the Strategy when you have a lot of similar classes that only differ in the way they execute some behavior.

- Use the pattern to isolate the business logic of a class from the implementation details of algorithms that may not be as important in the context of that logic.

- Use the pattern when your class has a massive conditional statement that switches between different variants of the same algorithm.

## Reference

- Video from Christopher Okhravi: https://www.youtube.com/watch?v=v9ejT8FO-7I

- Book: Design Patterns – Elements of Reusable Object-Oriented Software (1995 Addison-Wesley) by 
Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides

- Refactoring Guru Website: https://refactoring.guru/design-patterns/strategy

- Refactoring Guru Website: https://refactoring.guru/design-patterns/strategy/java/example

- Book: Head First Design Patterns – Building Extensible & Maintainable Object-Oriented Software (2021 O'REILLY) by Eric Freeman, Elisabeth Robson, Kathy Sierra and Bert Bates
