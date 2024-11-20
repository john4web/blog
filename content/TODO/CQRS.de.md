+++
title = "CQS und CQRS: Trennung von Lesen und Schreiben"
date = "2024-11-20"
description = ""
+++

# CQS und REST Endpoints

Man kann dieses Konzept auch sehr gut auf die Arbeit mit REST-Endpoints ummünzen. Man könnte sagen, dass man sich an folgende Konvention hält: Man definiert nur Endpoints, die entweder eine query operation oder eine command operation ausführt aber nicht beides zusammen. Wenn man an CRUD-Funktionalitäten denkt, sind CREATE, UPDATE und DELETE Command Operationen. Hingegen wäre READ eine Query-Operation.

Man könnte auch sagen, dass bei den folgenden HTTP-Methoden von REST

GET (Fordert Ressource vom Server an)
POST (Erstellt neue Ressource am Server)
PUT (Ändert die ganze Ressource am Server)
DELETE (Löscht eine Ressource vom Server)
PATCH (Ändert nur Teile der Ressource am Server)

Lediglich die GET Methode der Query-Operation entspricht. Alle anderen Methoden wie POST, PUT, DELETE und PATCH sind Command-Operationen, weil sie den State der Applikation verändern. Wenn man also CQS auch für REST-Endpoints anwenden möchte, dann muss man gewährleisten, dass ein Endpoint nicht Daten zurückliefert und gleichzeitig auch den State in irgendeiner Weise verändert. Das sollte dann in zwei unterschiedliche Endpoints aufgesplittet werden.