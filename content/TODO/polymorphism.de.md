+++
title = "Polymorphie und dynamische Bindung"
date = "2024-01-24"
description = ""
+++

## Überblick

Polymorphie ist neben Kapselung und Vererbung ein fundamentales Grundkonzept der objektorientierten Programmierung. Doch was ist Polymorphie genau? Und wann/wo sollte man polymorphen Code verwenden? Diese Blog-Artikel versucht diese Fragen zu beantworten.

In OOP ist jedes Objekt einer abgeleiteten Klasse auch gleichzeitig ein Objekt seiner Superklasse. Das heißt: Objektvariablen der Superklasse kann man Objekte der Subklasse zuweisen -> dadurch wird das Objekt "polymorph" (= Vielgestaltig).

Das folgende Codebeispiel zeigt ein polymorphes Objekt:

```java
Tier meinTier = new Hund();
```
In dieser Zeile wird ein Objekt der Klasse `Hund` erstellt, und in einer Objektvariable vom Typ `Tier` gespeichert. Dies ist möglich weil die Klasse Hund von der Klasse Tier erbt. Dementsprechend IST ein Hund auch ein Tier und die Variable vom Typ Tier kann auch ein Objekt vom Typ Hund enthalten. Dies nennt man Polymorphie, weil die Referenz vom Typ der Superklasse (Tier) auf ein Objekt der Subklasse (Hund) zeigt. Da meinTier zur Laufzeit sowohl ein Hund-Objekt als auch ein Katze-Objekt sein kann, ist es polymorph, also vielgestaltig.


## Objekte umwandeln

```java
Tier meinTier = new Hund(); // meinTier ist ein Hund
Hund meinHund = (Hund) meinTier;  // Casting von Tier zu Hund funktioniert
```

```java
Tier meinTier = new Katze(); // meinTier ist eine Katze
Hund meinHund = (Hund) meinTier;  // Fehler! Das führt zu einer ClassCastException
```

```java
Dog myDog = new Dog();      // Ein Hund-Objekt
Animal myAnimal = myDog;    // Automatisch in die Oberklasse (Animal) konvertiert
```


## Upcasting und Downcasting

https://www.youtube.com/watch?v=iAg3HIUHYx4
https://www.youtube.com/watch?v=Qpz2MA4KE9U  --> Bestes Video im Internet
 



https://www.youtube.com/watch?v=HpuH7n9VOYk&pp=ygUXamF2YSBwb2x5bW9ycGhpZSBjYXN0ZW4%3D
https://www.youtube.com/watch?v=Qb3YVlxwZwY 
https://www.youtube.com/watch?v=4yxtYwW2x4o
https://www.youtube.com/watch?v=zJjkyoeq9fg&pp=ygUXamF2YSBwb2x5bW9ycGhpZSBjYXN0ZW4%3D
https://www.youtube.com/watch?v=FNJCSazPkrA
https://www.youtube.com/watch?v=Q8cTydJSawQ

https://www.youtube.com/watch?v=j9QfB4Sf1pA

https://www.youtube.com/watch?v=jhDUxynEQRI

https://www.youtube.com/watch?v=1ONhXmQuWP8


folgende zwei sachen aus meiner SDM-Mitschrift einbauen: 
- Typecasting Zettel
- Which assignments are valid/invalid

## Wenn ich ein Interface als Datentyp verwende und ein Objekt zuweise - ist das dann auch Polymorphie?

## Polymorphie auf einem Blick
Das folgende Bild beschreibt das gesamte Konzept der Polymorphie auf einem einzigen von mir angefertigten Bild:

{{< figure src="/images/polymorphism/polymorphie.jpg" caption="Polymorphie auf einen Blick.">}}


## Dynamische Bindung

Es wird zur Laufzeit entschieden, welche der beiden Methoden (schreiben() der Superklasse oder schreiben() der Subklasse) aufgerufen werden soll.
Die JVM entscheidet zur Laufzeit auf Basis des Objekttyps, welche Methode verwendet wird.

## Wann sollte man polymorphen Code verwenden?

With examples

Folgende Videos unbedingt anschauen:

https://www.youtube.com/watch?v=YaSMkzmc_sA


Auch schreiben über "Static polymorphism vs. dynamic polymorphism".