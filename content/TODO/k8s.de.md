+++
title = "Einführung in Kubernetes"
date = "2026-03-17"
description = ""
+++

Im Folgenden werden die Erkenntnisse aus dem Buch "Kubernetes: Eine kompakte Einführung" simpel zusammengefasst.

## Warum verwendet man Kubernetes?

Kubernetes vereinfacht das Bauen, Deployen und Warten verteilter Systeme radikal. Durch die Konzepte (Immutabilität, deklarative Konfiguration und selbstheilende systeme) wird die Schnelligkeit (mit der zuverlässig Software deployed werden kann) radikal verbessert. Auch die Agilität und Effizienz wird erhöht. Mehr dazu im Folgenden:

### Immutabilität

Kubernetes folgt dem Prinzip der immutablen Infrastruktur. Was bedeutet das?

Anstatt ein bestehendes System zu verändern (z.B. durch Updates), wird einfach ein neues System gebaut und das alte System durch das neue System ersetzt.

Sobald ein Artifakt einmal im System erzeugt wurde, ändert es sich nicht mehr durch die Anwender. Es gibt keine inkrementellen Änderungen.

**Beispiel:**  
Eine laufende Instanz (z. B. Container, VM, Pod) wird nicht nachträglich verändert. Wenn sich etwas ändern muss (z.B. neue Version, Konfiguration, Bugfix), erstellt man ein neues Image
und ersetzt die alte Instanz komplett. Statt auf einem Server ein Update zu installieren, wird ein neues Container-Image gebaut und neue Pods werden gestartet.

**Vorteil der immutabilität:**
Das Bauen eines neuen Images anstatt ein bestehendes zu verändern sorgt dafür, dass das alte Image immer noch verfügbar ist. Wenn plötzlich etwas nicht mehr funktioniert, kann man einfach wieder das alte Image verwenden. Dadurch vermeidet man komplizierte Reparaturen am laufenden System!

Immutable Container-Images bilden den Kern von allem, was man in Kubernetes baut. Moderne Cloud-Systeme sind stabiler und einfacher zu betreiben, wenn man laufende Systeme nicht verändert, sondern immer neu erzeugt und ersetzt!

### Deklarative Konfiguration

Gehen wir von folgendem Fall aus:
**current state:** ich habe 5 Instanzen eines Microservices.
**desired state:** ich will 15 Instanzen eines Microservices haben.

Um den desired state zu erreichen, gibt es nun 2 verschiedene Herangehensweisen:

1. **imperative style**  
Ich habe aktuell 5 instanzen und gebe den Befehl _"add 10 instances"_  
Hier wird der gewünschte Status der Anwendung durch das Ausführen einer Folge von Anweisungen hergestellt.

2. **declarative style**  
Man spezifiziert nur den _"desired state"_. Man sagt dem Tool, dass man am Ende 15 Instanzen haben möchte.
Das tool checkt dann den current state, sieht dass aktuell 5 Instanzen da sind und fügt 10 Instanzen hinzu.

Kubernetes funktioniert im **deklarativen Stil**! Alles in Kubernetes ist ein _"deklaratives Konfigurations-Objekt"_, das den gewünschten Status des Systems repräsentiert. Es ist dann die Aufgabe von Kubernetes, dass der aktuelle Status der Wirklichkeit mit dem gewünschten Status übereinstimmt.

In Kubernetes erstellt man ein deklaratives _"configuration yaml file"_.
Darin sagt man nur: ich möchte 15 Instanzen in Production haben. Man macht sich keine Gedanken drüber wie viele Instanzen aktuell live sind. Kubernetes regelt das selber. Kubernetes vergleicht den "desired state" mit dem "current state", findet den Unterschied raus und führt die Befehle aus, um den "desired state" zu erreichen.

Während imperative Befehle "Aktionen" definieren, definieren deklarative Konfigurationen einen Status.
**Beispiel einer Aufgabe: 3 Instanzen erzeugen**
Imperatives Vorgehen: "Füre A aus!", "Führe B aus!", "Führe C aus!"
Deklaratives Vorgehen: "Anzahl an Instanzen gleich 3"

**Vorteile der deklarativen Konfiguration:**
- weniger Fehleranfällig, weil die Auswirkungen einer deklarativen Konfiguration auch ohne Ausführung verstanden werden können.
- es können Versionsverwaltung, Code-Reviews und Unit-Tests verwendet werden. (deklarative Konfiguration in Versionsverwaltung wird als "Infrastructure as Code" bezeichnet)
- Rollback von Änderungen sind einfach.

### Self-Healing Systems

**Prinzip:** Ein System sollte Fehler automatisch erkennen und selbst beheben, ohne dass ein Mensch eingreifen muss.

Kubernetes achtet kontinuierlich darauf, dass der aktuelle Status und der gewünschte Status weiterhin übereinstimmt. Das bedeutet Kubernetes initialisiert nicht nur das System sondern schützt es auch vor Fehlern oder Störungen, die es eventuell destabilisieren oder die Zuverlässigkeit beeinträchtigen können. 

Hätte man das nicht, müsste das ein Mensch manuell machen, der mitten in der Nacht aufgeweckt wird, langsamer reagiert, weniger zuverlässig ist und auch dafür bezahlt werden muss.

**Beispiel:** Aktuell laufen 3 Instanzen. Kubernetes legt nicht nur diese Instanzen an, sondern stellt auch sicher, dass es immer genau 3 Instanzen gibt. Erstellt man manuell eine vierte, wird Kubernetes eine zerstören, um die Anzahl wieder auf 3 zu verringern. Beendet man manuell eine Instanz, erzeugt Kubernetes eine, um den gewünschten Status zu erreichen.

"Self-Healing Systems" verbessern die Developer-Geschwindigkeit, weil weniger Zeit und Energie mehr für das Warten der Systeme aufgewendet werden muss.

### Service & Team Skalieren

**1. Entkoppeln**  
Kubernetes nutzt eine _"entkoppelte Architektur"_. Komponenten eines Systems sollen möglichst unabhängig voneinander funktionieren, sodass Änderungen oder Ausfälle einer Komponente nicht direkt andere Komponenten beeinflussen.

Jede Komponente ist von anderen Komponenten durch definierte APIs und Service-Load-Balancer getrennt. APIs und Load Balancer isolieren jedes Teil des Systems von den anderen. APIs dienen als Puffer zwischen dem Implementierer und dem Konsumenten und die Load Balancer liefern den Puffer zwischen den laufenden Instanzen jedes Service. 

Mit "Puffer" ist eine Zwischenschicht gemeint, die zwei Systemteile voneinander isoliert, damit Änderungen oder Ausfälle nicht direkt weitergegeben werden.

Diese entkoppelte Architektur erleichtert die Skalierung des Systems und des Developer-Teams.

**2. Einfaches Skalieren**  

Anwendungen und Cluster können super skaliert werden, weil:

- Anzahl an Instanzen sind einfach eine Zahl in der deklarativen Konfiguration!
- Es gibt soetwas wie "Autoscaling", das alles automatisch macht!
- Hinzufügen von Ressourcen ist einfacher durch Kubernetes!

**3. Microservices**  

Microservices wurden zu einem defacto Standard in der Softwareentwicklung. Meistens sind die Entwicklerteams in Unternehmen so zusammengestellt, dass jedes Team jeweils einen Microservice betreut. Mit Kubernetes ist es sehr leicht das Zusammenspiel von Microservices zu managen und dadurch kann man auch Teams besser skalieren.

Kubernetes stellt eine Reihe von Abstraktionen und APIs bereit, die es leicht machen, diese entkoppelte Microservice-Architektur zu bauen:

1. **Pods:** Sind Gruppen von (Docker-)Containern. Sie können von verschiedenen Teams entwickelte Container-Images zu einer einzelnen deploybaren Einheit verbinden.

2. **Kubernetes-Services:** Bieten "Load-Balancing", Naming und Discovery, um einen Microservice von anderen zu isolieren.

3. **Namensräume:** Bieten Isolation und Zugriffskontrolle, sodass jeder Microservice steuern kann, inwieweit andere Services mit ihm interagieren können.

4. **Ingress-Objekte:** Dienen als einfach zu nutzendes Frontend, das mehrere Microservices zu einer einzelnen, externalisierten API kombinieren kann.

**4. Seperation of Concerns**

Kubernetes teilt das System bewusst in viele kleine, klar abgegrenzte Komponenten auf. Jede Komponente hat genau eine Aufgabe. Das sorgt für hohe Konsistenz und ermöglicht, dass ein kleines Team, das einen Kubernetes-Cluster betreut, für die Unterstützung tausender von Teams - welche ihre Anwendungen in diesem Cluster laufen lassen - verantwortlich sein kann.

### Infrastruktur abstrahieren

Durch ein Aufbauen auf Kubernetes' anwendungsorientierter Abstraktion wird sichergestellt, dass die Aufwände für das Bauen, Deployen und Managen der Anwendung über einen großen Bereich von Umgebungen hinweg **portabel** sind.

### Effizienz
Durch Kubernetes lassen sich die Aufgaben von mehreren Anwendungen enger auf wenigen Maschinen zusammenfassen. Dadurch wird weniger Energie bzw. Rechenleistung verbraucht. Kosten für Serverbetrieb und Personal werden gespart. Auch das Deployen und testen von Anwendungen wird effizienter und günstiger.

## Container erstellen und ausführen

Im folgenden wird folgendes erklärt:
- Wie eine Anwendung mithilfe des Docker-Image-Formats verpackt wird
- Wie eine Anwendung mit der Docker-Container-Runtime gestartet wird

### Container-Images

...

## Quellen:

- https://www.youtube.com/watch?v=67E2JRk8_bg