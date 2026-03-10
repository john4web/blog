+++
title = "Einführung in Kubernetes"
description = ""
+++


Im Folgenden werden die Erkenntnisse aus dem Buch "Kubernetes: Eine kompakte Einführung" simpel zusammengefasst.

## Warum verwendet man Kubernetes?

Kubernetes vereinfacht das Bauen, Deployen und Warten verteilter Systeme radikal. Durch die folgenden 3 Konzepte (Immutabilität, deklarative Konfiguration und selbstheilende systeme) wird die Schnelligkeit, mit der zuverlässig Software deployed werden kann, radikal verbessert.

### Immutabilität

Kubernetes folgt dem Prinzip der immutablen Infrastruktur. Was bedeutet das?

Anstatt ein bestehendes System zu verändern (z.B. durch Updates), wird einfach ein neues System gebaut und das alte System durch das neue System ersetzt.

Sobald ein Artifakt einmal im System erzeugt wurde, ändert es sich nicht mehr durch die Anwender. Es gibt keine inkrementellen Änderungen.

Beispiel:
Eine laufende Instanz (z. B. Container, VM, Pod) wird nicht nachträglich verändert. Wenn sich etwas ändern muss (neue Version, Konfiguration, Bugfix), erstellt man ein neues Image
und ersetzt die alte Instanz komplett. Statt auf einem Server ein Update zu installieren, wird ein neues Container-Image gebaut und neue Pods werden gestartet.

Vorteile der immutabilität:
1. Durch erstellen eines neuen Artifaktes, sieht man genau, was sich im Vergleich zum letzten Artifakt verändert hat (z.B. via Git-Commit)
2. Das Bauen eines neuen Images anstatt ein bestehendes zu verändern sorgt dafür, dass das alte Image immer noch verfügbar ist. Wenn plötzlich etwas nicht mehr funktioniert, kann man einfach wieder das alte Image verwenden. Dadurch vermeidet man komplizierte Reparaturen am laufenden System!

Immutable Container-Images bilden den Kern von allem, was man in Kubernetes baut.
Moderne Cloud-Systeme sind stabiler und einfacher zu betreiben, wenn man laufende Systeme nicht verändert, sondern immer neu erzeugt und ersetzt!


### Deklarative Konfiguration

current state: ich hab 5 Instanzen eines Microservices
desired state: ich will 15 Instanzen eines Microservices haben

1. imperative style
ich habe aktuell 5 instanzen und gebe ein "command" "add 10 instances"

Hier wird der gewünschte Status der Anwendung durch das Ausführen einer Folge von Anweisungen hergestellt.


2. declarative style
you specify the **end state** only. you tell the tool, that you wanna have 15 instances at the end.
das tool checkt den current state, sieht dass aktuell 5 instances da sind und fügt 10 Instanzen hinzu

Kubernetes funktioniert im deklarativen Stil. Alles in Kubernetes ist ein _"deklaratives Konfigurations-Objekt"_, das den gewünschten Status des Systems repräsentiert. Es ist dann die Aufgabe von Kubernetes, dass der aktuelle Status der Wirklichkeit mit dem gewünschten Status übereinstimmt.

z.B. in kubernetes erstellt man ein declarative configuration yaml file
you only say: i wanna have 15 instances in production. you will not worry about how many instances are currently live. kubernetes will handle it on its own. Kubernetes vergleicht den desired state und den actual state, findet den Unterschied raus und führt die commands aus um den end state zu erreichen.



Während imperative Befehle aktionen definieren, defineiren deklarative Konfigurationen einen Status.

Beispiel einer Aufgabe: 3 Instanzen erzeugen
Imperatives Vorgehen: "Füre A aus!", "Führe B aus!", "Führe C aus!"
Deklaratives Vorgehen: "Anzahl an Instanzen gleich 3"


Vorteile der deklarativen Konfiguration:
- weniger Fehleranfällig, weil die Auswirkungen einer deklarativen Konfiguration auch ohne Ausführung verstanden werden können.
- es können Versionsverwaltung, Code-Reviews und Unit-Tests verwendet werden. (deklarative Konfiguration in Versionsverwaltung wird als "Infrastructure as Code" bezeichnet)
- Rollback von änderungen sind einfach


### Self-Healing Systems

Prinzip: Ein System sollte Fehler automatisch erkennen und selbst beheben, ohne dass ein Mensch eingreifen muss.

Kubernetes achtet kontinuierlich darauf, dass der aktuelle Status und der gewünschte Status weiterhin übereinstimmt. Das Bedeutet Kubernetes initialisiert nicht nur das System sondern schützt es auch vor Fehlern oder Störungen, die es eventuell destabilisieren oder die Zuverlässigkeit beeinträchtigen können. 

Hätte man das nicht, müsste das ein Mensch manuell machen, der mitten in der Nacht aufgeweckt wird, langsamer reagiert, weniger zuverlässig ist und auch dafür bezahlt werden muss.

Beispiel: Aktuell laufen 3 Instanzen. Kubernetes legt nicht nur diese Instanzen an, sondern stellt auch sicher, dass es immer genau 3 Instanzen gibt. Erstellt man manuell eine vierte, wird Kubernetes eine zerstören, um die Anzahl wieder auf 3 zu verringern. Beendet man manuell eine Instanz, erzeugt Kubernetes eine, um den gewünschten Status zu erreichen.

Self-Healing Systems verbessern die Developer-Geschwindigkeit, weil keine Zeit und Energie mehr für das Warten der Sytse

### Service & Team Skalieren

**Entkoppeln**
Kubernetes nutzt eine _"entkoppelte Architektur"_. Komponenten eines Systems sollen möglichst unabhängig voneinander funktionieren, sodass Änderungen oder Ausfälle einer Komponente nicht direkt andere Komponenten beeinflussen.

Jede Komponente ist von anderen Komponenten durch definierte APIs und Service-Load-Balancer getrennt. APIs und Load Balancer isolieren jedes Teil des Systems von den anderen. APIs dienen als Puffer zwischen dem Implementierer und dem Konsumenten und die Load Balancer liefern den Puffer zwischen den laufenden Instanzen jedes Service. 

Mit "Puffer" ist eine Zwischenschicht gemeint, die zwei Systemteile voneinander isoliert, damit Änderungen oder Ausfälle nicht direkt weitergegeben werden.

Diese entkoppelte Architektur erleichtert die Skalierung des Systems und des Developer-Teams.

**2**
**3**
**4**

### Infrastruktur abstrahieren

### Effizienz



https://www.youtube.com/watch?v=67E2JRk8_bg