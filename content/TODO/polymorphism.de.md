+++
title = "Polymorphie und dynamische Bindung"
date = "2024-01-24"
description = ""
+++

## Überblick

Polymorphie ist neben Kapselung und Vererbung ein fundamentales Grundkonzept der objektorientierten Programmierung. Doch was ist Polymorphie genau? Und wann/wo sollte man polymorphen Code verwenden? Diese Blog-Artikel versucht diese Fragen zu beantworten. Grundsätzlich gibt es zwei Arten der Polymorphie - statische und dynmaische Polymorphie.

## Statische Polymorphie (Compile-Time Polymorphism)

Das ist normales überladen von Methoden. Also wenn ich eine Klasse habe, in der ich zwei Methoden mit demselben Namen aber unterschiedlicher Parameterliste definiere, dann ist das "Compile-Time Polymorphism". Hier entscheidet der Compiler, welche der beiden Methoden aufgerufen werden soll. Der Compiler unterscheidet die verschiedenen Methodenvarianten anhand der Anzahl und der Typisierung ihrer Parameter.

## Dynamische Polymorphie (Run-Time Polymorphism)

Bei der dynamischen Polymorphie kommt nun noch ein zweites Mittel zum Tragen - nämlich das überschreiben von Methoden von Superklassen in Subklassen. Aber starten wir mit einem Beispiel.

In OOP ist jedes Objekt einer abgeleiteten Klasse auch gleichzeitig ein Objekt seiner Superklasse. Das heißt: Objektvariablen der Superklasse kann man Objekte der Subklasse zuweisen -> dadurch wird das Objekt "polymorph" (= Vielgestaltig). Warum ist das möglich? Weil die Superklasse und die Subklasse sich das gleiche _"Interface"_ teilen.

Das folgende Codebeispiel zeigt ein polymorphes Objekt:

```java
Animal myAnimal = new Dog();
```
In dieser Zeile wird ein Objekt der Klasse `Dog` erstellt, und in einer Objektvariable vom Typ `Animal` gespeichert. Dies ist möglich weil die Klasse Dog von der Klasse Animal erbt. Dementsprechend IST ein Dog auch ein Animal und die Variable vom Typ Animal kann auch ein Objekt vom Typ Dog enthalten. Dies nennt man Polymorphie, weil die Referenz vom Typ der Superklasse (Animal) auf ein Objekt der Subklasse (Dog) zeigt. Da myAnimal zur Laufzeit sowohl ein Dog-Objekt als auch ein Cat-Objekt sein kann, ist es polymorph, also vielgestaltig.

Das folgende Bild beschreibt das gesamte Konzept der dynamischen Polymorphie auf einem einzigen von mir angefertigten Bild:

{{< figure src="/images/polymorphism/polymorphie.jpg" caption="Polymorphie auf einen Blick.">}}

## Dynamische Bindung

Was bedeutet nun _"Dynamische Bindung"_? In dem vorher genannten Beispiel wird zur Laufzeit entschieden, welche der beiden Methoden (``schreiben()`` der Superklasse oder ``schreiben()`` der Subklasse) aufgerufen werden soll. Die JVM entscheidet also zur Laufzeit auf Basis des Objekttyps, welche Methode verwendet wird. Das nennt man dynamische Bindung.

## Implizite Typumwandlung

```java
// Mountainbike erbt von Object
Mountainbike m = new Mountainbike();
Object obj = m;

// primitive Datentypen
int i = 5;
double d = i;
```

## Explizite Typumwandlung

```java
// Mountainbike erbt von Object
Object obj = new Object();
Mountainbike bike = (Mountainbike) obj;

// primitive Datentypen
double d = 7.321;
int i = (int) d;
```

## Upcasting und Downcasting

Gehen wir davon aus die Klasse Dog erbt von der Klasse Animal.

Der folgende Code beschreibt "Upcasting". Dies funktioniert automatisch in Java. Es wird ein Objekt hergenommen und zur Superklasse "hochgecastet".

```java
Animal animal = new Dog();
```

De folgende Code wirft einen Error, weil man nicht direkt eine Animal-Referenz einer Dog-Referenz zuweisen kann, ohne sie vorher downzucasten.

```java
Dog myDog = animal; // ERROR!
```

Der folgende Code beschreibt "Downcasting". Es wird ein Objekt hergenommen und zur Subklasse "runtergecastet".

```java
Dog myDog = (Dog) animal;
```

Es ist ratsam, vor dem Downcasting den instanceof-Operator zu verwenden, um sicherzustellen, dass animal tatsächlich ein Dog ist, um eine ClassCastException zur Laufzeit zu vermeiden:

```java
if (animal instanceof Dog) {
    Dog myDog = (Dog) animal; // Sicheres Downcasting
}
```

Manche Leute finden übrigens, dass Downcasting ein Code Smell ist. Das findet z.B. auch Christopher Okhravi, der ein interessantes Video zu diesem Thema gemacht hat: https://www.youtube.com/watch?v=ECTyHSZUv0c . Er argumentiert damit, dass Downcasting Verträge im Code verletzen könnte und auch das Open Closed Principle verletzt wird.

## Polymorphie mittels Interfaces

ToDo!!

Die Anwendung der Polymorphie mittels interfaces ist wohl die bekannteste Methode. 

Wenn ich ein Interface als Datentyp verwende und ein Objekt zuweise - ist das dann auch Polymorphie?

Wenn du ein Interface als Datentyp verwendest und einem Objekt eines Typs, der dieses Interface implementiert, einen Wert zuweist, nutzt du Polymorphie. Hier ist eine kurze Erklärung, wie das funktioniert:

## Die 3 Grundeigenschaften von OOP (oder vielleicht doch nur 1?)

Man hört es überall - die 3 Grundeigenschaften der objektorientierten Programmierung sind Kapselung, Vererbung und Polymorphie. Ich weiß nicht wer das ursprünglich so definiert hat aber diese Aussage ist in aller Munde. Sogar während meinem Studium haben das die Professoren immer wieder gesagt und in der Software Engineering Community gilt dies mittlerweile als Standard-Aussage die glaube ich mittlerweile schon von vielen Leuten getroffen wird, ohne darüber wirklich nachzudenken. Oft wird auch auf die Frage: _"Was ist OOP eigentlich?"_ die Antwort _"OOP besteht aus Kapselung, Vererbung und Polymorphie._". Auch ich dachte das viele Jahre, bevor ich das Buch "Clean Architecture" von Robert C. Martin gelesen habe. In Kapitel 5 des Buches (Object-Oriented Programming ab S. 33) beschreibt Martin, dass es die Konzepte Kapselung und Vererbung schon lange vor der Entwicklung von objektorientierten Sprachen gab. Er schreibt z.B. davon, dass Kapselung in einer perfekten Art und Weise bereits in der Programmiersprache C möglich war. In Sprachen wie Java oder C# ist Kapselung nur in einer abgeschwächten Art und Weise möglich, da es hier unmöglich ist, die Deklaration und die Definition einer Klasse voneinander zu trennen.
Vererbung konnte man in C auch schon praktizieren (wenn zwar nur mit einem Trick). Durch objektorientierte Sprachen wurde Vererbung zwar simpler und angenehmer für die/den Programmierer/in gestaltet aber das Konzept war schon vorher da.
Auch polymorphie konnte bereits in C praktiziert werden und ist nichts neues in objektorientierten Sprachen jedoch machen sie die Anwendung von Polymorphie sehr viel einfacher. Laut Robert C. Martin:

_Using an OO language makes polymorphism trivial. That fact provides an enormous power that old C programmers could only dream of. On this basis, we can conclude that OO imposes discipline on indirect transfer of control._

Martin beschreibt Polymorphie als besonders mächtig, weil sie die Abhängigkeiten zwischen Komponenten entkoppelt und es ermöglicht, Systeme modularer und leichter erweiterbar zu gestalten. Stichwort: "Plugin-Architecture".

Laut Robert C. Martin:

Clean-Architecture S. 47
_OO is the ability, through the use of polymorphism, to gain absolute control over every source code dependency in the system. It allows the architect to create a plugin architecture, in which modules that contain high-level policies are independent of modules that contain low-level details. The low-level details are relegated to plugin modules that can be deployed and developed independently from the modules that contain high-level policies._

Aus diesem Zitat schließe ich, dass die simple Anwendung des Dependency-Inversion Principles (DIP) das fundamentalste Merkmal von objektorientierter Programmierung ist. Und dieses Principle baut auf der Verwendung von Polymorphie auf. Ohne Polymorphie gibt es kein DIP. Deshalb würde ich persönlich sagen es gibt nur eine einzige Grundeigenschaft von OOP. Und das ist Polymorphie.

# Reference

## Books

- Robert C. Martin: _Clean Architecture: A Craftsman's Guide to Software Structure and Design (Robert C. Martin Series)_, Publisher: Pearson, Year: 2017, ISBN: 0134494164

## Videos

- https://www.youtube.com/watch?v=W2pTtlNaa6s

- https://www.youtube.com/watch?v=HpuH7n9VOYk

- https://www.youtube.com/watch?v=ECTyHSZUv0c