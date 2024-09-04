+++
title = "Das Strategy Pattern"
date = "2024-01-24"
description = "Blog-Artikel, der das Strategy Design Pattern der Gang of Four erklärt."
+++

This blog post offers an explanation of Strategy Design Pattern by Gang of Four. The examples are written in Java.

{{< figure src="/images/strategy-pattern/duck.jpg" caption="A duck can fly and quack (and can do probably much more things). [Image-Source: Pixabay](https://pixabay.com/photos/duck-mallard-bird-pond-plumage-8510483/)" >}}


## The Problem that the strategy pattern solves

Let's say we have a `Duck` class. And then we have a `CityDuck` (living in the city) and a `WildDuck` (living in the forest). We use inheritance – so `CityDuck` and `WildDuck` inherit from `Duck`. The subclasses are responsible for implementing their own version of the `display()` method. Wild-ducks can display themselves differently than city-ducks for instance. So they have to override the `display()` method. But the `quack()` method is shared amongst all these subclasses. Inheritance is used here for code reuse.

{{< figure src="/images/strategy-pattern/1.svg" caption="`CityDuck` and `WildDuck` inherit from `Duck`" >}}

What is the problem here? The problem is **CHANGE** (which happens everytime). The only constant in software development is change! When requirements change, our current software design may not necessarily be appropriate for the incoming requirements. An example for such a change could be the following:

What if we have another method, for example `fly()`? Because ducks can fly. We add the method to the base class `Duck`. But then you want to introduce a new class called `RubberDuck` which inherits from `Duck`. `RubberDuck` has its own `display()` method. But rubber-ducks can't fly. Now we have a problem! rubber-ducks can't fly however they inherit the `fly()` method from their parent class `Duck` → which is wrong! ⚠️


{{< figure src="/images/strategy-pattern/2.svg" caption="`RubberDuck` inherits the `fly()` method! Now everyone who is using a `RubberDuck` object can call the `fly()` method on it which should not be possible because rubber-ducks can't fly.⚠️ " >}}

And what if we wanna add a new class called `MountainDuck` (which lives in the mountains). This duck implements its own `display()` method. But it implements also its own `fly()` method because the duck has a specific way to fly (ultrafast for instance).
And then we add a new class called `CloudDuck`. this duck also overrides `display()` and `fly()` methods. But the cloud-duck flys EXACTLY the same as the mountain-duck. What are we doing now? We cannot reuse the same code from the mountain-duck. So we have to copy and paste. And that is very, very bad! ⚠️ (cf. DRY Principle)

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

In the following, the full duck example is displayed in UML. 

{{< figure src="/images/strategy-pattern/6.svg" caption="One example of the strategy pattern in action. " >}}


## Intent of the strategy pattern

The original GoF Design-Pattern Book states the intent as follows:  
**_"Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it."_**

In the following, I will try to explain each phrase of the intent individually. Note! In this original intent the term  _"family of algorithms"_ means the same as "strategy". In the following, the word strategy is mainly used instead of _"family of algorithms"_.

**_"Family of algorithms"_:**  
in diesem Beispiel sind die Familien IFlyBehavior und IQuackBehavior

**_"Algorithms"_:**  
Sind in diesem Fall die Verhaltensweisen (= konkreten implementierungen der Strategy). Das sind LowFlying, Highflying, LoudQuacking und QuietQuacking. LowFlying und Highflying gehören zur Familie IFlyBehavior. LoudQuacking und QuietQuacking gehören zur Familie IQuackBehavior.

**_"Strategy lets the algorithm vary independently from clients that use it"_:**    
Jede "concrete Strategy Class" hat ihr eigenes, unabhängiges Verhalten. Diese konkreten implementierungen der Familien IFlyBehavior und IQuackBehavior können komplett unterschiedlich zueinander sein.

**_"make them interchangeable"_:**  
Konkrete Behaviors können ganz leicht ausgetauscht werden (solange sie dasselbe Interface implementieren).
z.B. kann man eine neue Klasse "FastFlying" einführen, die "IFlyBehavior" implementiert. Und schon kann man der duck1 das Verhalten `new FastFlying()` anstatt dem Verhalten `new LowFlying()` übergeben. We decouple the algorithm from the one that is using the algorithm. Whoever is using the algorithm is not forced to change when you are changing one of the algorithms.


## When should you use this pattern?

- Use the Strategy pattern when you want to use different variants of an algorithm within an object and be able to switch from one algorithm to another during runtime.

- Use the Strategy when you have a lot of similar classes that only differ in the way they execute some behavior.

- Use the pattern to isolate the business logic of a class from the implementation details of algorithms that may not be as important in the context of that logic.

- Use the pattern when your class has a massive conditional statement that switches between different variants of the same algorithm.

## Reference

- Video from Christopher Okhravi:
https://www.youtube.com/watch?v=v9ejT8FO-7I

- Book: Design Patterns – Elements of Reusable Object-Oriented Software (1995 Addison-Wesley) by 
Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides

- Refactoring Guru Website: https://refactoring.guru/design-patterns/strategy

- Refactoring Guru Website: https://refactoring.guru/design-patterns/strategy/java/example

- Book: Head First Design Patterns – Building Extensible & Maintainable Object-Oriented Software (2021 O'REILLY) by Eric Freeman, Elisabeth Robson, Kathy Sierra and Bert Bates
