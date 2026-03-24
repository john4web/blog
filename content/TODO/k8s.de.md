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

Im folgenden wird erklärt:
- Wie eine Anwendung mithilfe des Docker-Image-Formats verpackt wird
- Wie eine Anwendung mit der Docker-Container-Runtime gestartet wird

### Docker-Images

Ein Container-Image ist ein Binary-Paket. Es fasst alle Dateien zusammen, die für das Ausführen eines Programms in einem OS-Container notwendig sind. Das Image ist keine einzelne Datei, sondern eher eine Spezifikation für ein manifest-file, das auf andere Dateien verweist.

Das verbreitetste Format für Container-Images ist das _Docker Image Format_. Es wurde für das verpacken, verteilen und ausführen von Containern entwickelt. Es besteht aus einer Reihe von Schichten (Dateisystem-Layer). Jeder Layer übernimmt den vorherigen Layer und passt ihn an:

.
├── Container A: nur Basis-Betriebssystem, wie z.B. Debian
│   ├── Container B: baut auf A auf, fügt Ruby v2.1.10 hinzu
        └── Container D: baut auf B auf, fügt Rails v4.2.6 hinzu
        └── Container E: baut auf B auf, fügt Rails v3.2.x hinzu

Ein neuer Layer wird in Docker immer dann erzeugt, wenn eine Anweisung im Dockerfile ausgeführt wird, die das Dateisystem verändert. Die meisten Dockerfile-Instuktionen (wie z.B. RUN, COPY, ADD, etc.) erzeugen jeweils einen neuen Layer. 

Es gibt zwei Arten von Container:

1. **System-Container**  
Diese Container versuchen VMs nachzubilden. Sie beinhalten eine Reihe von Systemdiensten (z.B. ssh, cron und syslog). Mit der Zeit wurde das aber "Bad Practice" und man verwendete dann eher "Anwendungscontainer" stattdessen.

2. **Anwendungs-Container**  
Diese Container lassen nur ein einzelnes Programm in sich laufen. Das ist eine gute Design-Philosophie und eignet sich gut für das Zusammenstellen skalierbarer Anwendungen.

Ein Dockerfile wird dafür genutzt, das Erstellen eines Image zu automatisieren. Das Dockerfile ist ein "Rezept" für das Bauen eines Images.

Bei Images sollte man darauf achten, dass man die Image-Größe optimiert und auch bei der Image-Sicherheit keine Abstriche macht. Es sollten niemals Container gebaut werden, in denen Passwörter enthalten sind. Secrets und Images sollten niemals vermischt werden! Und Achtung: das Löschen einer Datei in dem einen Layer entfernt die Datei nicht aus vorigen Layern!

Wie Docker-Files aufgebaut sind und wie man images baut und ausführt, ist in meinem anderen Blogpost zum Thema Docker genau erklärt!

#### Multistage Image Builds

Große images entstehen z.B. wenn das Kompilieren des Programms Teil der Konstruktion des Container-Images ist. Damit verbleiben aber unnötigerweise alle Entwicklungstools im finalen Image/Container und blasen es auf. Um dieses Problem zu lösen gibt es docker multistage-builds. Dabei wird aus einem Dockerfile nicht einfach ein einzelnes Image erstellt sondern mehrere. Jedes image wird als Stage angesehen und Artefakte können aus vorigen Stages in den aktuellen Stage kopiert werden. Dadurch kann die Größe des Image um hunderte von Megabytes verkleinert und die Deploy-Zeit drastisch verkürzt werden.

#### Images in Remote-Registry ablegen

_Was nützt einem ein Image, wenn es nur auf einem Rechner verfügbar ist?_

Deshalb pusht man es in ein Repository. Es gibt öffentliche Repositories (wie z.B. DockerHub) oder private Repositories (wie z.B. JFrog Artifactory oder Sonatype Nexus).

#### Container-Ressourcen begrenzen

Mit docker kann man auch die Ressourcen begrenzen, die von Anwendungen genutzt werden können. Dazu nutzt es die cgroup-Technologie des Linux-Kernels.

Im Folgenden wird mit den Flags --memory und --memory-swap ein container auf 200MB Speicher und 1GB Swap-Speicher begrenzt. Nutzt das Programm zu viel Speicher, wird es beendet:

```bash
docker run -d \
  --name my-container \
  --publish 8080:8080 \
  --memory 200m \
  --memory-swap 1G \
  <image-name>
```

Die CPU kann man mit dem --cpu-shares flag einschränken:

```bash
docker run -d \
  --name my-container \
  --publish 8080:8080 \
  --memory 200m \
  --memory-swap 1G \
  --cpu-shares 1024 \
  <image-name>
```

#### Images Aufräumen

Um Docker Images aus dem lokalen Repository zu löschen, wird der Befehl "docker rmi" verwendet:

```bash
docker rmi <tag-name>
```

oder

```bash
docker rmi <image-id>
```

Mit dem Befehl "docker images" kann man sich die auf dem Rechner verfügbaren images anzeigen lassen.

## Basic Kubernetes Components
ToDo: 
Bilder einfügen

### Node
Ein Node (oder auch oft Worker-Node genannt) ist ein einfacher Server (also eine physische oder virtuelle Maschine).

### Pod
_"Smallest unit of Kubernetes"_

Ein Pod ist eine Abstraktion über einem Container (also ein zusätzlicher layer über dem container)

Warum gibt es diese Pods? Weil Kubernetes die Container-Runtime abstrahieren möchte -> diese kann dann zum Beispiel ausgetauscht werden. Außerdem muss man sich als Kubernetes-Entwickler nicht mehr mit Containern beschäftigen, sondern man muss sich nur mehr um Pods kümmern.

Normalerweise läuft in jedem Pod immer genau 1 Docker Container. Es ist zwar möglich, mehrere Container in einem Pod auszuführen, aber das ist unüblich.

Kubernetes stellt ein virtuelles Netzwerk bereit, was bedeutet, dass jeder Pod seine eigene IP-Adresse erhält (nicht der Container, sondern der Pod erhält die IP-Adresse!). Jeder Pod kann über diese IP-Adresse mit anderen Pods kommunizieren.

Aber Pods sind "ephemeral" – also sie können sehr leicht "sterben". Wenn ein Container stirbt, weil die Anwendung darin abgestürzt ist, stirbt der Pod und ein neuer Pod wird an seiner Stelle erstellt. Dabei wird dem neuen Pod eine neue IP-Adresse zugewiesen.

In diesem Fall müsste man die IP-Adresse jedes Mal anpassen, wenn der Pod neu startet. Weil die Pods ja über ihre IP-Adressen miteinander kommunizieren und wenn sich die Adresse plötzlich ändert, muss man die Adresse in den anderen Pods anpassen. Das ist sehr umständlich. Deshalb gibt es die Kubernetes-Komponente „Service“.

### Service
Ein Service ist im Grunde genommen eine statische/permanente IP-Adresse mit einem DNS-Namen, die an jeden Pod angehängt werden kann. Wenn der Pod stirbt, bleiben der Service und seine IP-Adresse bestehen, sodass man die IP-Adresse nicht anpassen muss, wenn ein Pod neu erstellt wird. Pods kommunizieren über über diese Services. Ein Service fungiert übrigens auch als Load-Balancer

### Ingress
Man möchte ja, dass die Applikationen öffentlich aufrufbar sind. Zum Beispiel, wenn in einem Pod eine REST-API steckt, will man ja dass diese öffentlich unter einer URL verfügbar ist. Und die URL soll auch einen schönen Domain-Name haben und z.B. auch ein sicheres Protokoll (https) verwenden. Genau dafür gibt es die Ingress-Komponente. Wenn von Außen ein Request an die REST-API eingeht, dann geht dieser zuerst an den Ingress und dieser Ingress leitet den Request weiter zum Service und dieser leitet den Request weiter an den Pod usw.

Ingress ist also dazu da, HTTP/HTTPS-Anfragen von außen in den eigenen Cluster zu steuern - also quasi ein intelligenter Einstiegspunkt für Web-Traffic. Er leitet externe Anfragen (z.B. von Browsern, etc.) an die richtigen Services im Cluster weiter und ermöglicht Routing basierend auf Hostnamen oder URLs.

### ConfigMap

Man stelle sich folgendes Beispiel vor:  
Man hat einen Pod, der eine Applikation beinhaltet und einen Pod, der eine DB beinhaltet. Die Applikation muss auf die DB zugreifen. Die Kommunikation läuft dabei über Services ab. Normalerweise hat man in der Applikation im application.yml file die JDBC-URL zur Datenbank drinnen. Also die JDBC-URL ist innerhalb des gebauten images der Applikation enthalten. Würde sich diese JDBC-URL ändern, müsste man das im application.yml file ändern, die Applikation mit einer neuen Version rebuilden, das image zum Repo pushen, in den Pod pullen und das ganze restarten. Das ist umständlich! Dafür gibt es ConfigMaps! Eine ConfigMap ist eine externe Konfiguration für die Applikation. In diesem Fall würde in der ConfigMap die JDBC-Url drinnenstehen und die ConfigMap ist dann mit dem Pod verknüpft, sodass der Pod die Daten aus der ConfigMap bekommt. Wenn man in diesem Fall die JDBC-URL ändern möchte, braucht man die URL nur in der ConfigMap ändern und das war's! In der Applikation selber kann man auf die Einträge der ConfigMap dann z.B. via Environment-Variables oder ein property-file zugreifen.
Achtung! Datenbank-Passwort, Username oder andere "Credentials" sollte man keinesfalls in ConfigMaps geben. Dafür gibt es nämlich eine andere Komponente namens "Secret".

### Secret

Die Secret-Komponente funktioniert genauso wie die ConfigMap-Komponente, nur mit dem Unterschied, dass es verwendet wird um geheime Daten zu speichern. Die darin gespeicherten Credentials sind base64 encoded. Passwörter, Zertifikate, usw. sind Beispiele für Daten, die man hier reinlegen sollte. 

### Volumes

Wenn man z.B. einen Pod mit einer Datenbank drinnen hat, und der Pod restarted wird, sind die Daten weg, weil der Pod "ephemeral" ist. Das ist ein Problem weil man will die Daten ja behalten. Und das macht man via Volumes. 

Volumes binden einen physischen Speicher auf einer Festplatte an den Pod. Dieser Speicher kann sich auf einer lokalen Maschine befinden (auf demselben Worker-Node, auf dem der Pod läuft) oder auf einem externen Speicher außerhalb des Kubernetes-Clusters (Cloud-Speicher, eigener On-Premises-Speicher usw.).

Storage sollte man sich immer als externes hard-drive plug-in im Kubernetes-Cluster vorstellen. Weil Kubernetes kümmert sich explizit NICHT um Datenpersistenz! Der Kubernetes-Administrator ist selber dafür verantwortlich, ein Backup der Daten anzulegen, die Daten zu replizieren und zu managen, und schauen dass sie sicher sind.  

### Deployments

Wenn z.B. eine Applikation crasht und der Pod restarted wird, hat man eine "Down-Time", in der die Applikation nicht erreichbar ist. Das sollte (vor allem in Production) niemals passieren! Um das zu vermeiden, repliziert man alles auf verschiedenen Workernodes. Also man klont den gesamten workernode als "Replica". Das "Replica" ist dann mit dem selben service verbunden! Services sind auch load-balancer. Der service ist also mit mehreren Pods aus verschiedenen WorkerNodes verbunden und leitet die Requests dann an jene Pods weiter, die am wenigsten mit Anfragen beschäftigt sind. Wenn also ein Pod stirbt, leitet der Service die Anfrage an einen anderen Pod weiter.

Um ein "Replica" zu erstellen, erstellt man aber nicht einfach den Pod doppelt sondern man definiert eine "Blaupause" für einen bestimmten Pod und spezifiziert darin die Anzahl der Replicas. Und genau diese Blaupause ist die Deployment-Komponente.

Ein Deployment ist also eine Blaupause für einen bestimmten Pod. In der Praxis erstellt man keine Pods sondern Deployments. Darin kann man viele Sachen für den Pod einstellen.

- Ein Pod ist eine Abstraktion über einem Container
- Ein Deployment ist eine Abstraktion über Pods

Deployments machen es einfach, die Pods zu konfigurieren. In der Praxis arbeitet man also mit Deployments statt Pods.

### Stateful Sets

Auch für Datenbanken müssen Replicas angelegt werden. Weil wenn ein DB-Pod down ist, muss eine andere stattdessen verfügbar sein. 
ABER: Datenbanken können nicht via "Deployments" repliziert werden. Warum? Weil Datenbanken einen "State" haben (der State sind die Daten in der DB). Die Replicas müssen ja jeweils dieselben Daten ausliefern und den selben State haben, damit das funktioniert. Sie müssen alle auf denselben shared data storage zugreifen. Dafür braucht man einen Mechanismus, der managt, welche Pods gerade auf diesen Storage schreiben und welche gerade von diesem Storage lesen um Dateninkonsistenzen zu vermeiden. Dieser Mechanismus wird von der Stateful-Sets Komponente angeboten. StatefulSets werden für Statful Applications verwendet wie z.B. Datenbanken aller Art, ElasticSearch, oder irgendwelche anderen Applikationen, die einen State aufweisen. Diese Unterscheidung ist sehr wichtig! Für stateless Applications werden "Deployments" verwendet und für stateful Applications werden "Stateful Sets" verwendet.

StatefulSets kümmern sich – ähnlich wie Deployments – um die Replikation der Pods und deren Hoch- oder Herunterskalierung, stellen dabei jedoch sicher, dass Lese- und Schreibvorgänge der Datenbank synchronisiert werden, sodass keine Inkonsistenzen in der Datenbank entstehen.

Das ist übrigens ziemlich schwer zu managen weshalb es auch eine common practice ist, die Datenbanken außerhalb vom Kubernetes Cluster zu hosten und nur stateless applications innerhalb vom Kubernetes Cluster zu haben.

### Namespaces

Innerhalb eines Kubernetes-Clusters kann man Ressourcen in Namespaces organisieren. In einem Cluster kann man mehrere Namespaces haben. Namespaces kann man sich wie einen virtuellen Cluster in einem Cluster vorstellen.

Kubernetes liefert 4 Default-Namespaces out of the box:
- **kube-system**: darin sollte man nichts erstellen oder modifizieren. Darin sind Systemprozesse drinnen.
- **kube-public**: Enthält die öffentlich zugängliche Daten und Cluster informationen
- **kube-node-lease**: Enthält Informationen zum "Herzschlag" von Nodes und bestimmt die Verfügbarkeit von Nodes.
- **default**: Ressourcen, die man am Anfang erstellt, landen hier. Man kann aber auch eigene namespaces erstellen.

Namespaces verwendet man, um den Überblick im Kubernetes Cluster zu behalten, indem man Ressourcen gruppiert.

## Kubernetes Architecture

ToDo:
mit bildern erklären
https://www.youtube.com/watch?v=umXEmn3cMWY
https://www.youtube.com/watch?v=TlHvYWVUZyc
https://www.youtube.com/watch?v=vmCuNJiNzfg
https://www.youtube.com/watch?v=8C_SCDbUJTg

## Kubernetes Client
ToDo:
Inhalte vom Buch einfügen

## Cluster Componenten
ToDo:
Inhalte vom Buch einfügen


## Quellen:

https://www.youtube.com/watch?v=Krpb44XR0bk
https://www.youtube.com/watch?v=K3jNo4z5Jx8