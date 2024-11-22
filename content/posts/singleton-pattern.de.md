+++
title = "Das Singleton Pattern - Ein Code-Smell?"
date = "2024-01-01"
description = "Artikel, der das Singleton-Entwurfsmuster der Gang of Four erklärt."
+++

Dieser Blogbeitrag bietet eine Erklärung des Singleton-Entwurfsmusters der Gang of Four. Die Beispiele sind in Java geschrieben.

{{< figure src="/images/singleton-pattern/singleton-comic.png" caption="Ein Comic, der das Singleton-Muster darstellt (Source: [Refactoring-Guru](https://refactoring.guru/design-patterns/singleton)). Das Singleton-Muster stellt sicher, dass es nur eine einzige Instanz eines Objekts gibt. Diejenigen, die darauf zugreifen, merken möglicherweise nicht einmal, dass sie ständig mit demselben Objekt arbeiten." >}}

# Intent

Das ursprüngliche GoF-Designmusterbuch definiert das Singleton-Muster wie folgt: **Ensure a class only has one instance, and provide a global point of access to it.**


In anderen Worten:
Das Singleton Pattern wird dann verwendet, wenn man von einer Klasse __nur eine einzige__ Instanz erzeugen möchte. Man möchte also verbieten, mehrere Instanzen einer Klasse zu erzeugen.


You have a class and singleton pattern helps you to make it impossible to instantiate that class except for a single time and whenever you want an instance, you would inevitably have to use that instance. Whenever you ask for an instance, you always get the same instance.

Many people argue that Singleton is an "anti pattern" or "code smell". But why?

Sie haben eine Klasse, und das Singleton-Pattern hilft Ihnen, es unmöglich zu machen, diese Klasse mehr als einmal zu instanziieren. Wann auch immer Sie eine Instanz benötigen, müssen Sie zwangsläufig diese eine Instanz verwenden. Jedes Mal, wenn Sie nach einer Instanz fragen, erhalten Sie immer dieselbe Instanz.

Viele Menschen argumentieren, dass das Singleton-Muster ein "Anti-Pattern" oder "Code-Smell" ist. Aber warum?

# Hauptkritikpunkte:

1. Singletons sind globale Variablen und es ist wichtig, globale Variablen in der Softwareentwicklung zu vermeiden. Denn wann immer etwas global zugänglich ist, ist es schwierig zu kontrollieren, und es könnte sich ändern, ohne dass man es weiß. Jeder im gesamten Programm könnte es ändern.

2. _"Die falsche Annahme"_: Man geht davon aus, dass man in der Zukunft immer nur eine einzige Instanz dieser bestimmten Klasse benötigt. Das ist nicht unbedingt wahr, insbesondere wenn Ihre Anwendungen wachsen. Man kann das nicht im vorhinein sicher wissen.

Es ist völlig in Ordnung, innerhalb einer Anwendung nur ein einziges Objekt einer Klasse zu haben. Aber es ist nicht in Ordnung, es unmöglich zu machen, eine zweite Instanz zu erstellen. Denn man weiß nicht, ob man (zu einem späteren Zeitpunkt) eine zweite Instanz benötigen wird.

Es gibt einen interessanten [Google TechTalk](https://www.youtube.com/watch?v=-FRm3VPhseI) von Miško Hevery (der übrigens der Schöpfer des AngularJS-Frameworks ist) über Singletons und ihren globalen Zustand. In seinem Vortrag beschreibt er, dass Singletons wirklich ein Albtraum sind, und begründet seine Meinung mit Codebeispielen. Um den Vortrag zusammenzufassen:
- Singletons machen Code untestbar.
- Jedes Singleton generiert eine unendliche Anzahl zusätzlicher globaler Variablen, weil alles, was für eine globale Variable zugänglich ist, ebenfalls global ist.

Ich teile ebenfalls diese Meinungen und betrachte das Singleton-Pattern eher als Code-Smell anstatt als Pattern, das tatsächlich verwendet werden sollte. Aber werfen wir einen genaueren Blick auf das Pattern:

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

Die statische Methode `getInstance()` kann von außen aufgerufen werden. Beim ersten Aufruf wird eine neue Singleton Instanz erzeugt. Um sicherzustellen, dass immer diese eine Instanz verwendet wird, wird sie in der Variable `instance` gespeichert. Diese ist statisch.

In der Main-Methode kann man dann folgendes machen:

```java
Singleton s = Singleton.getInstance();
```
 
# Quellen

## Video
- Video von Christopher Okhravi: _Singleton Pattern – Design Patterns (ep 6)_, URL: https://www.youtube.com/watch?v=hUE_j6q0LTQ, Letzter Aufruf: 14.10.2024.

## Bücher
- Erich Gamma, Richard Helm, Ralph Johnson and John Vlissides: _Design Patterns – Elements of Reusable Object-Oriented Software_, Verlag: Addison-Wesley, Jahr: 1995, ISBN: 0201633612

- Eric Freeman, Elisabeth Robson, Kathy Sierra and Bert Bates: _Head First Design Patterns: Building Extensible and Maintainable Object-Oriented Software 2nd Edition_, Verlag: O'REILLY, Jahr: 2021, ISBN: 9781492078005

## Webseite
- Refactoring Guru Webseite: _Singleton_, URL: https://refactoring.guru/design-patterns/singleton, Letzter Aufruf: 14.10.2024.

## Vortrag
- Google TechTalk von Miško Hevery: _The Clean Code Talks: Global State and Singletons_, Talk-Datum: 13.11.2008, URL: https://www.youtube.com/watch?v=-FRm3VPhseI, Letzter Aufruf: 14.10.2024.
 
