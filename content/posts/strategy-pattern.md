+++
title = "The Strategy Pattern"
date = "2024-01-24"
description = "Article explaining the Strategy Design Pattern by Gang of Four."
+++

This blog post offers an explanation of Strategy Design Pattern by Gang of Four. The examples are written in java.

{{< figure src="/images/strategy-pattern/duck.jpg" caption="A duck can fly and swim (and probably much more things). [Image-Source: Pixabay](https://pixabay.com/photos/duck-mallard-bird-pond-plumage-8510483/)" >}}

## Intent

_"Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it."_

Note! In this original intent the term  _"family of algorithms"_ mean the same as "Strategy". In the following, the word strategy is mainly used instead of _"family of algorithms"_.

_"Family of algorithms"_: in diesem Beispiel sind die Familien IFlyBehavior und IQuackBehavior

_"algorithms"_: Sind in diesem Fall die Verhaltensweisen (= konkreten implementierungen der Strategy). Das sind LowFlying, Highflying, LoudQuacking, QuietQuacking.  LowFlying und Highflying gehören zur Familie IFlyBehavior. LoudQuacking und QuietQuacking gehören zur Familie IQuackBehavior.

_"Strategy lets the algorithm vary independently from clients that use it"_: Jede "concrete Strategy Class" hat ihr eigenes, unabhängiges Verhalten. Diese konkreten implementierungen der Familien IFlyBehavior und IQuackBehavior können komplett unterschiedlich zueinander sein.

_"make them interchangeable"_:
Konkrete Behaviors können ganz leicht ausgetauscht werden (solange sie dasselbe Interface implementieren).
z.B. kann man eine neue Klasse "FastFlying" einführen, die "IFlyBehavior" implementiert. Und schon kann man der duck1 das Verhalten `new FastFlying()` anstatt dem Verhalten `new LowFlying()` übergeben. We decouple the algorithm from the one that is using the algorithm. Whoever is using the algorithm is not forced to change when you are changing one of the algorithms.

## Overview
Verhaltensweisen werden aus einer Klasse (Context) ausgelagert in andere Klassen (Concrete Strategies) und via Interfaces lose an die Klasse (Context) gekoppelt. Die Kontext Klasse ruft dann lediglich den Code der konkreten Strategies auf. Dadurch können Objekte und deren Verhaltensweisen komplett easy zusammengebaut werden:

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

{{< figure src="/images/strategy-pattern/strategy.jpg" caption="Objekte werden mit Verhaltensweisen zusammengebaut (lose gekoppelt)" >}}

Das Strategy-Pattern ist ein Paradebeispiel dafür, dass Composition besser ist als Inheritance. We need Inheritance much much less than we believe.

The concrete implementations (LoudQuacking, QuietQuacking, etc.) can vary just as much as they want - without having to change the code of the context Class (Duck). And that is the power of the strategy pattern!

## The Problem that the strategy pattern solves


Let's say we have a duck class. And then we have a City Duck (living in the city) and a Wild Duck (living in the forest).
We use inheritance. The subclasses are responsible for implementing their own version of the display() method. Wild ducks can display differently than city ducks for instance. So they have to override the display method. But the quack method is shared amongst all these subclasses. Inheritance is used here for code reuse.

What is the problem? The problem is change. (Which happens everytime). The only constant in Software development is Change! When requirements change our curren software design may not necessarily be appropriete for the incoming requirements. 


{{< figure src="/images/strategy-pattern/1.jpg" caption="" >}}

What if we have another method like fly()? Because ducks can fly. We add the method to the base class Duck. But then you want to introduce a new class called rubberduck which inherits from duck. rubberduck has its own display method. But rubberducks can't fly. Now we have a problem. Problem 1!!!


{{< figure src="/images/strategy-pattern/2.jpg" caption="" >}}

And what if we wanna add a new class called MountainDuck (which lives in the mountains). This duck implements its own display method. But it implements also his own fly method because the duck has a specific way to fly (ultrafast for instance).
And then we add a new class called CloudDuck. this duck also overrides display and fly methods. But the CloudDuck flys EXACTLY the same as the Mountain duck. What are we doing now? We cannot reuse the same code from the mountain duck. So we have to Copy and paste. And that is very, very bad. (See: DRY Principle)
Problem 2!!!

{{< figure src="/images/strategy-pattern/4.jpg" caption="" >}}

Some might say you could solve some of these problems with "more inheritance". You could for instance introduce a new base class just for mountain and cloud duck where the both ducks are inheriting from. But the more classes you create and inherit from, the more "inflexible" all of that gets. 

The problem is, that you can't share behaviour over classes that are in the same hierarchy (horizontally):
{{< figure src="/images/strategy-pattern/3.jpg" caption="" >}}
Inheritance only works by sharing code from top to bottom.


The solution to problems with inheritance is not "more inheritance". The solution is composition! And that's exactly what the strategy pattern does.

Please help us Strategy Pattern!!!!



## The Pattern itself
sdsdfsdf

{{< figure src="/images/strategy-pattern/5.jpg" caption="" >}}

{{< figure src="/images/strategy-pattern/6.jpg" caption="" >}}

## The Code one by one

sdfdf

## Reference

- Big Shout out to Christopher Okhravi who has explained the strategy pattern so easily, that everybody can understand it:
https://www.youtube.com/watch?v=v9ejT8FO-7I

- Book: Design Patterns - Elements of Reusable Object-Oriented Software (1995 Addison-Wesley) by 
Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides

- https://refactoring.guru/design-patterns/strategy

- https://refactoring.guru/design-patterns/strategy/java/example

- Head First Design Patterns Book






