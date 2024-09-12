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

Und was, wenn wir eine neue Klasse namens `MountainDuck` (die in den Bergen lebt) hinzufügen wollen? Diese Ente implementiert ihre eigene `display()`-Methode. Aber sie implementiert auch ihre eigene `fly()`-Methode, weil diese Ente auf eine bestimmte Art fliegt (zum Beispiel ultraschnell). Und dann fügen wir eine neue Klasse namens `CloudDuck` hinzu. Diese Ente überschreibt ebenfalls die Methoden `display()` und `fly()`. Aber die `CloudDuck` fliegt EXAKT genauso wie die `MountainDuck`. Was machen wir jetzt? Wir können den gleichen Code der `MountainDuck` nicht wiederverwenden. Also müssen wir duplizieren (copy and paste). Und das ist sehr, sehr schlecht! ⚠️ (Vgl. DRY-Prinzip)

{{< figure src="/images/strategy-pattern/3.svg" caption="" >}}

Manche würden wshl. an dieser Stelle sagen, dass man einige dieser Probleme mit "mehr Vererbung" lösen könnte. Zum Beispiel könnte man eine neue Basisklasse einführen, von der `MountainDuck` und `CloudDuck` erben. Aber je mehr Klassen man erstellt von denen wiederum andere erben, desto "unflexibler" wird das alles.

Das Problem dabei ist, dass man kein Verhalten zwischen Klassen in derselben Hierarchie (horizontal) teilen kann. Vererbung funktioniert nur, indem Code von oben nach unten weitergegeben wird.

{{< figure src="/images/strategy-pattern/4.svg" caption="Wenn es um Vererbung geht, kann Code nicht zwischen Klassen geteilt werden, die sich in derselben Vererbungshierarchie befinden." >}}

Die Lösung für Probleme mit Vererbung ist nicht "mehr Vererbung". Die Lösung ist **Composition**! Und genau das macht das Strategy-Pattern.

## Überblick
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

Das Strategy-Pattern ist ein Paradebeispiel dafür, dass Composition besser ist als Inheritance. Wir brauchen Vererbung viel weniger, als wir denken.

Die konkreten Implementierungen (LoudQuacking, QuietQuacking etc.) können sich beliebig voneinander unterscheiden, ohne dass der Code der Kontextklasse (Duck) geändert werden muss. Und genau das ist die Stärke des Strategy-Patterns!

## Das Pattern selbst
So sieht das vollständige Pattern in UML (Unified Modeling Language) aus:

{{< figure src="/images/strategy-pattern/5.svg" caption="UML Diagramm des Strategy Patterns." >}}

Die `Context`-Klasse hat ein oder mehrere Objekte vom Typ Strategy als Membervariable. Die Membermethoden der `Context`-Klasse rufen dann einfach die `behaviour()`-Methoden auf, die in den `ConcreteStrategy`-Klassen implementiert sind.

Im Folgenden wird das vollständige Enten-Beispiel in UML dargestellt. Im Gegensatz zum vorherigen UML-Diagramm zeigt dieses Beispiel, wie das Pattern mit mehreren Strategys (`IFlyBehaviour` und `IQuackBehaviour`) funktioniert:

{{< figure src="/images/strategy-pattern/6.svg" caption="Ein Beispiel für das Strategy-Pattern in Action." >}}

## Definition des Strategy-Patterns

Das ursprüngliche GoF-Design-Pattern-Buch definiert das Strategy-Pattern wie folgt:
**_"Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it."_**

In the following, I will try to explain each phrase of the intent individually. Note! In this original intent the term  _"family of algorithms"_ means the same as "strategy". In the following, the word strategy is mainly used instead of _"family of algorithms"_.

Im Folgenden werde ich versuchen, jede Phrase dieser Definition einzeln zu erklären. Hinweis: In dieser ursprünglichen Definition bedeutet der Begriff _"Familie von Algorithmen"_ dasselbe wie "Strategy". Im Folgenden wird hauptsächlich das Wort Strategy anstelle von _"Familie von Algorithmen"_ verwendet.

**_"Family of algorithms"_:**  
In diesem Beispiel können die Klassen IFlyBehavior und IQuackBehavior als "Familien" angesehen werden.

**_"Algorithms"_:**  
Sind in diesem Fall die Verhaltensweisen (= konkreten implementierungen der Strategy). Das sind LowFlying, Highflying, LoudQuacking und QuietQuacking. LowFlying und Highflying gehören zur Familie IFlyBehavior. LoudQuacking und QuietQuacking gehören zur Familie IQuackBehavior.

**_"Strategy lets the algorithm vary independently from clients that use it"_:**    
Jede "Concrete Strategy Class" hat ihr eigenes, unabhängiges Verhalten. Diese konkreten implementierungen der Familien IFlyBehavior und IQuackBehavior können komplett unterschiedlich zueinander sein.

**_"make them interchangeable"_:**  
Konkrete Behaviors können ganz leicht ausgetauscht werden (solange sie dasselbe Interface implementieren).
z.B. kann man eine neue Klasse "FastFlying" einführen, die "IFlyBehavior" implementiert. Und schon kann man der `duck1` das Verhalten `new FastFlying()` anstatt dem Verhalten `new LowFlying()` übergeben. Hier wird der Algorithmus von demjenigen entkoppelt, der den Algorithmus verwendet. Wer auch immer den Algorithmus verwendet, ist nicht gezwungen, Änderungen vorzunehmen, wenn einer der Algorithmen geändert wird.

## Wann sollte man das Strategy-Pattern verwenden?

- Verwende das Strategy-Pattern, wenn du unterschiedliche Varianten eines Algorithmus innerhalb eines Objekts nutzen und zur Laufzeit von einem Algorithmus zu einem anderen wechseln möchtest.

- Verwende das Strategy-Pattern, wenn du viele ähnliche Klassen hast, die sich nur darin unterscheiden, wie sie ein bestimmtes Verhalten ausführen.

- Verwende das Pattern, um die Business-Logic einer Klasse von den Implementierungsdetails der Algorithmen zu isolieren, die im Kontext dieser Logik möglicherweise nicht so wichtig sind.

- Verwende das Pattern, wenn deine Klasse ein riesiges "Conditional Statement" enthält, das zwischen verschiedenen Varianten desselben Algorithmus wechselt.

## Quellen

- Video von Christopher Okhravi: https://www.youtube.com/watch?v=v9ejT8FO-7I

- Buch: Design Patterns – Elements of Reusable Object-Oriented Software (1995 Addison-Wesley) by 
Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides

- Buch: Head First Design Patterns – Building Extensible & Maintainable Object-Oriented Software (2021 O'REILLY) by Eric Freeman, Elisabeth Robson, Kathy Sierra and Bert Bates

- Refactoring Guru Website: https://refactoring.guru/design-patterns/strategy

- Refactoring Guru Website: https://refactoring.guru/design-patterns/strategy/java/example
