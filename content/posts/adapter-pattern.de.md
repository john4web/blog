+++
title = "Das Adapter Pattern"
date = "2024-07-15"
description = "Artikel, der das Gang of Four Adapter-Design-Pattern erklärt."
+++

Dieser Blog Artikel erklärt das GoF Adapter Design Pattern. Die Beispiele wurden in Java verfasst.

{{< figure src="/images/adapter-pattern/adapter.png" caption="Visualisierung des Adapter-Patterns (Source: [Refactoring-Guru](https://refactoring.guru/design-patterns/adapter))" >}}

# Definition
Das ursprüngliche GoF-Design-Pattern-Buch definiert das Adapter-Pattern wie folgt: **_"Converts the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces."_**

**Mit anderen Worten:** Das Adapter-Pattern macht zwei Schnittstellen kompatibel, die nicht kompatibel sind.

# Das Problem

Das Verhalten des Adaptees ist das eigentliche Verhalten, das ich haben möchte. Aber ich kann dieses Verhalten mit der vorhandenen Schnittstelle, die ich habe, nicht bekommen. Wie löse ich das?

# Das Pattern

{{< figure src="/images/adapter-pattern/plug.jpg" caption="Skizze des Adapter-Patterns. Zwei Schnittstellen (der Stecker und die Steckdose) sind inkompatibel. Ein Adapter wird verwendet, um sie kompatibel zu machen." >}}

{{< figure src="/images/adapter-pattern/adapter_pattern.jpg" caption="Beispiel für das Adapter-Pattern. Der Client verwendet einen Adapter, um die Methode specificRequest() des Adaptees aufzurufen." >}}

In diesem Beispiel ist die Methode `request()` die Methode, die der Client **erwartet**. Diese Methode hat eine Signatur, und der Client erwartet es, diese Signatur aufzurufen. Der Adapter nimmt den `request()`-Aufruf entgegen, delegiert ihn jedoch an eine andere Klasse (Adaptee), die einen völlig anderen Vertrag befolgt.

Der Client (= Benutzer des Codes) hat etwas vom Typ `ITarget`. Dieses ITarget hat eine Methode namens `request()`. Und `request` ist der Standard, an den der Client gewöhnt ist. Aber da der Client etwas verwenden möchte, das eine völlig andere Schnittstelle hat (den Adaptee), können wir `request()` nicht einfach aufrufen, weil der Adaptee keine Methode namens `request()` hat. Der Adaptee hat eine Methode namens `specificRequest()`. Was wir also tun, ist, die Methode `request()` aufzurufen - aber wir rufen sie auf einem Adapter auf. Der Adapter folgt der Signatur `request()`, an die wir gewöhnt sind. So können wir `request()` aufrufen und uns darauf verlassen, dass der Adapter den Adaptee aufruft. Der Adapter hat einen Adaptee und ist somit dafür verantwortlich, die Anfrage an den Adaptee weiterzuleiten, damit der Adaptee die spezifische Aktion ausführen kann, an der wir ursprünglich interessiert waren.

Mit anderen Worten:  
Der Client möchte aus irgendeinem Grund die Methode `specificRequest()` aufrufen, aber aus einem anderen Grund möchte er sie mit der spezifischen Signatur von `request()` aufrufen.

The client couples to an interface called `ITarget` that follows the particular signature of `request()`. And then we have a concrete implementation of this interface called Adapter which follows this signature and when the method `request()` is called, then we call (stupidly and blindly) the method `specificRequest` using this other signature. So we stick the Adapter in between the Client and the Adaptee so that they are compatible.

Der Client koppelt an eine Schnittstelle namens `ITarget`, die der Signatur von `request()` folgt. Und dann haben wir eine konkrete Implementierung dieses Interfaces (=Adapter), welche dieser Signatur folgt. Wenn die Methode `request()` aufgerufen wird, rufen wir (dumm und blind) die Methode `specificRequest` mit dieser anderen Signatur auf. Auf diese Weise platzieren wir den Adapter zwischen dem Client und dem Adaptee, damit sie kompatibel sind.

Der Client ruft die Methode `specificRequest()` **über** die Methode `request()` auf. Wir rufen die Methode `request()` auf dem Adapter auf, aber letztendlich bedeutet das, dass wir die Methode `specificRequest()` auf dem Adaptee aufrufen.

# Ziel des Patterns

Die Absicht des Adapter-Patterns besteht darin, das zugrunde liegende Verhalten nicht zu ändern. Die Absicht ist es nicht, Verhalten hinzuzufügen, zu entfernen oder zu verändern. Die Absicht ist es, die Anfrage gewissermaßen _„blind“_ weiterzugeben.

Man hat zwei verschiedene Arten von Signaturen/Schnittstellen, die nicht zusammen passen. Die Absicht ist es, sie interoperabel zu machen.

Dieses Design-Pattern hat keine intelligente Logik. Es delegiert die Aufrufe einfach dumm weiter. Es ist gewissermaßen wie ein "Vermittler" oder "Nachrichtenüberbringer", der die Nachrichten 1:1 delegiert.

# Ein einfacher Wrapper
Ein Adapter wird auch als Wrapper bezeichnet.  
Er "umschließt" etwas, damit man an dieses etwas andocken kann.

Warum ist es ein Wrapper? Das sieht man perfekt im Code des Clients, der den Adapter verwendet, um das Verhalten des Adaptees auszuführen:

```java
class Client {
    ITarget target = new Adapter(new Adaptee());
    target.request();
}
```
In der zweiten Zeile des obigen Codeausschnitts "umschließt" der Adapter den Adaptee.

# Ein praktisches Beispiel

Stellen Sie sich vor, Sie verwenden in einem Ihrer Projekte eine third-party library. In Ihrem Code rufen Sie eine bestimmte Methode dieser Bibliothek mit spezifischen Methodenparametern auf. Plötzlich wird die Bibliothek aktualisiert, und in der neuen Version der Bibliothek wird die Reihenfolge der Methodenparameter geändert. Was Sie jetzt tun könnten, ist, einen Adapter zu schreiben. Sie müssen die neue Reihenfolge der Methodenparameter in Ihrem Code nicht verwenden. Stattdessen können Sie einen Adapter verwenden, der die Methode mit der neuen Parameterreihenfolge aufruft. Auf diese Weise muss der Client-Code nicht geändert werden (übrigens erinnert mich das an das Open-Closed-Principle).

Denken Sie an das Wort _„Indirektion“_. Das Adapter-Pattern führt eine Ebene der Indirektion ein. Anstatt das Ding aufzurufen, das wir direkt aufrufen möchten, rufen wir etwas auf, das das Ding aufruft.

# Ein Dolmetscher
Man kann das Adapter Pattern auch mit einem Dolmetscher oder Vermittler vergleichen. Nehmen wir an, es gibt zwei personen, die unterschiedliche Sprachen sprechen. Dadurch können sie sich gegenseitig nicht verstehen. Wenn man aber einen Dolmetscher dazwischen schaltet, dann kann er die Sprachen übersetzen, sodass die beiden Parteien miteinander kommunizieren können. In diesem Szenario wäre der Übersetzer der Adapter, und die beiden Personen (welche die unterschiedliche Sprachen sprechen) sind der Client und der Adaptee.

# Quellen

## Video
- Video von Christopher Okhravi: _Adapter Pattern – Design Patterns (ep 8)_, URL: https://www.youtube.com/watch?v=2PKQtcJjYvc, Letzter Aufruf: 12.10.2024.

## Website
- Refactoring Guru Webseite: _Adapter_, URL: https://refactoring.guru/design-patterns/adapter, Letzter Aufruf: 12.10.2024.

