+++
title = "The Broken Windows Theory in Software Engineering"
date = "2024-01-24"
description = ""
+++

{{< figure src="/images/broken-windows/broken_windows.jpg" width="80%" caption="Foto by [Tim Arterbury](https://unsplash.com/de/@tim_arterbury?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/de/fotos/zwei-zerbrochene-glasfenster-5Uh-wTSz-q0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)">}}


When I read the book *[The Pragmatic Programmer](https://en.wikipedia.org/wiki/The_Pragmatic_Programmer)*, one particular image wouldn’t leave my mind: _broken window panes_. In the chapter “Software Entropy”, Andrew Hunt and David Thomas describe a phenomenon that also applies to software development — the famous “broken windows theory”.  

In this blog post, I will explain what this theory is all about and how it can help protect software from slowly decaying over time.

## Software Entropy

The word "Entropy" originates from physics and describes the level of disorder within a system. According to the laws of thermodynamics, this disorder naturally tends to increase over time. Entropy also manifests itself in software development. Just as in physical systems, disorder naturally grows over time, gradually causing software to decay. This is commonly known as “software rot”.  

Many factors can lead to software rot, but the most crucial one appears to be the psychology/mindset/culture, shaping the project. No matter how well a project is planned or how skilled the team, it can still fall into decay over time. Still, there are projects that, even in the face of major obstacles and persistent setbacks, succeed in defying the natural pull toward disorder and come out strong. But how?

## Broken Windows

In city neighborhoods, some buildings stay well-maintained and inviting, while others slowly crumble and fall apart. What causes this difference? Studies on urban decay and crime have identified a surprisingly small but powerful trigger that can turn a perfectly fine building into a neglected, abandoned structure in no time. The culprit? **A single broken window**.

Even one broken window, if left unfixed for a while, sends a powerful message to the building’s occupants: _"No one cares about this place!"_.
That small signal often sets off a chain reaction: more windows get damaged, trash starts piling up, graffiti appears, and structural problems begin to develop. In a surprisingly short time, the building can deteriorate to the point where repair seems pointless, and neglect becomes a reality.

The same thing applys to software! If you leave “broken windows” (bad designs, poor decisions, or sloppy code) unrepaired, the software will rot in no time!

Wenn die software immer mehr dahinrottet wird das instandhalten und warten mit der zeit so mühsam, dass man irgendwann dann sagt: es rentiert sich nicht mehr die software zu verbessern. Man muss sie neu bauen. Aber passiert das dann? Meistens nicht! Weil es mit enormen Kosten verbunden wäre. Weil sich kein entwickler die Mühe antun will das alles neu zu bauen. Oft handelt es sich dabei um riesengroße applikationen, wo man jahrelang braucht, um die Applikation von grund auf neu zu bauen. Oder der Kunde will einen Neubau der Applikation gar nicht erst bezahlen. In den Augen des Kunden funktioniert die software ja. Wenn dann eine winzige Änderung der Software ewig dauert und dadurch viel kostet, dann stört es den Kunden nicht so sehr - ja klar - er weiß ja nicht wie schnell ein neues Feature umgesetzt werden könnte, wenn die Software schön geschrieben wäre. Und so plagt man sich immer und immer weiter mit der verrotteten Software ab. Zum Leid der armen Entwickler, die sich dann mit fürchterlichem Code abplagen müssen, nichts mehr dazulernen, neuwertige features in verrotteten code einbauen müssen und letztenendes reputation verlieren, weil sie ewig dafür brauchen um kleinste Arbeiten umzusetzen. Und auch zum Leid der Firma selbst: So verliert die Firma immer mehr an Reputation und Geld usw.

Sehen sie meinen Punkt? So steuert man langsam aber doch auf einen abgrund zu. Sie denken sich jetzt vielleicht, dass ich nur schwarz male - aber das beschriebene Szenario ist für viele Firmen Realität! Es gibt firmen, denen genau das tag für tag passiert. Und am Anfang stand immer ein einziges zerbrochenes Fenster.

## Criminology

But where did this "Broken Window Theory" actually come from? Its origins are not in software development but in criminology. The Broken Window Theory led police in New York and other major cities to tackle minor problems before they escalate. By quickly addressing things like broken windows, graffiti, and small violations, they found that serious crimes could be prevented more effectively.

According to Wikipedia:

> In criminology, the broken windows theory states that visible signs of crime, antisocial behavior and civil disorder create an urban environment that encourages further crime and disorder, including serious crimes. The theory suggests that policing methods that target minor crimes, such as vandalism, loitering, public drinking and fare evasion, help to create an atmosphere of order and lawfulness<br>
> — <cite>[Wikipedia](https://en.wikipedia.org/wiki/Broken_windows_theory)</cite>


## How to fix Broken Windows
----------------------------------------------------------------
-- ab hier bis zum schluss: UMFORMULIEREN!!
-----------------------------------------------------------------------

Rule #1: Don’t leave “broken windows” (bad designs, wrong decisions, or poor code) unrepaired. Fix each one as soon as it is discovered.

Rule #2: If there is insufficient time to fix it properly, then board it up. Perhaps you can comment out the offending code, or display a "Not Implemented" message, or substitute dummy data instead. Take some action to prevent further damage and to show that you’re on top of the situation.

We’ve seen clean, functional systems deteriorate pretty quickly once windows start breaking. There are other factors that can contribute to software rot, but neglect accelerates the rot faster than any other factor.


Don’t let entropy win. If it wins, then you’d better plan on moving to another neighborhood

## Putting Out Fires

By contrast, there’s the story of an obscenely rich acquaintance of Andy’s. His house was immaculate, beautiful, loaded with priceless antiques, objets d’art, and so on. One day, a tapestry that was hanging a little too close to his living room fireplace caught on fire. The fire department rushed in to save the day—and his house. But before they dragged their big, dirty hoses into the house, they stopped—with the fire raging—to roll out a mat between the front door and the source of the fire.

They didn’t want to mess up the carpet.

A pretty extreme case, to be sure, but that’s the way it must be with software. One broken window—a badly designed piece of code, a poor management decision that the team must live with for the duration of the project—is all it takes to start the decline. If you find yourself working on a project with quite a few broken windows, it’s all too easy to slip into the mindset of “All the rest of this code is crap, I’ll just follow suit.” It doesn’t matter if the project has been fine up to this point.

In the original experiment leading to the “Broken Window Theory,” an abandoned car sat for a week untouched. But once a single window was broken, the car was stripped and turned upside down within hours.

By the same token, if you find yourself on a team and a project where the code is pristinely beautiful—cleanly written, well designed, and elegant—you will likely take extra special care not to mess it up, just like the firefighters. Even if there’s a fire raging (deadline, release date, trade show demo, etc.), you don’t want to be the first one to make a mess.



## Refutation of the counterarguments

- keiner hat die zeit ständig kaputte fenster in projekten zu fixen
Antwort: Aber hat man später dann die Zeit für den Mehraufwand wenn man neue Features einbauen will und man braucht ewig dafür?

- keiner hat bock ständig kaputte fenster in projekten zu fixen
Antwort1: Aber das ist genau deine Arbeit als Software Engineer
Antwort2: das kostet dich sicher weniger Mühe, als später eine komplett verrottete Applikation warten zu müssen.

- es rentiert sich nicht das anzugreifen: das muss irgendwann mal sowieso von grund auf neu geschrieben werden. "Das Haus nicht sanieren sondern gleich neu bauen".

- „Es funktioniert ja sowieso noch.“

Argument: Solange der Fehler die Funktionalität nicht direkt beeinträchtigt, ist es Zeitverschwendung, ihn jetzt zu beheben.

Realität: Auch kleine Probleme können sich summieren und später schwerwiegende Fehler verursachen („Software-Rot“).

- „Ich habe gerade keine Zeit dafür.“

Argument: Andere Aufgaben oder Deadlines haben Vorrang.

Realität: Aufgeschobene kleine Fehler werden oft später teurer und aufwendiger zu beheben.

- „Das stört doch niemanden.“

Argument: Fehler in weniger genutzten Modulen oder Bereichen sind irrelevant.

Realität: Über Zeit beeinflussen solche „kleinen Fenster“ die Codequalität, erschweren Wartung und erhöhen das Risiko von Bugs.

- „Ich will den Code nicht jetzt anfassen, ich plane größere Refactorings.“

Argument: Beheben würde später sowieso wieder überschrieben werden.

Realität: Kleine Verbesserungen stören selten größere Umbauten und verhindern, dass sich schlechte Gewohnheiten festsetzen.

- „Es ist nicht mein Problem.“

Argument: Fehler liegen außerhalb des eigenen Verantwortungsbereichs.

Realität: Softwareprojekte leben von gemeinsamer Verantwortung; Ignorierte Fehler belasten das Team langfristig.









In software development, later is the day that never comes.

{{< figure src="/images/broken-windows/later.jpeg" caption="Fixing stuff later in Software Engineering (Source: [Twitter Post from Vlad Mihalcea](https://x.com/vlad_mihalcea/status/1802901724362887580))." width="400">}}

Hier auch über "boiled frogs" vom Buch "Pragmatic Programmer" schreiben.
(S. 4 bis 9) und (S. 224 bis 225)

Auch drüber schreiben, dass man "Fenster", die man nicht sofort fixen kann, auch mit einem TODO kommentar versehen kann. Also ein "Achtung" schild markiert, das anzeigt dass es bald gefixt wird. Aber: nur todo kommentare mit Ticketnummern sind erlaubt. 

Hier die analogie einfügen mit der Rettung:

Wenn jemand auf der straße zusammenbricht, gehen alle leute vorbei und keiner fühlt sich zustänndig. Jeder der vorbeigeht denkt sich "das wird schon jemand anderer sehen bzw. da wird schon jemand anderer dafür verantwortlich sein diesem menschen zu helfen". Aber weil sich das jeder denkt, wird ihm niemand helfen.

Dasselbe ist in folgendem szenario: stell dir vor jemand bricht zusammen und hat einen herz-kreislauf stillstand. Dann geht jemand hin und macht CPR. Ebendiese Person screit dann "ruft die rettung"! Das ist das schlechteste was man machen kann. Wenn man alle rundherum gleichermaßen anspricht, dann fühlt sich wieder keine person zuständig und jeder denkt sich "wird schon jemand anderer machen". Eine bessere Methode wäre, eine dezidierte Person, welche in der Nähe rumsteht auszuwählen und sie direkt ansprechen. "Hey du! Du rufst jetzt die Rettung und sagst dem Disponent dass es sich um einen Herz-Kreislauf Stillstand handelt!". Dadurch fühlt sich eine Person direkt angesprochen und wird viel wahrscheinlicher handeln. Warum bringe ich jetzt diese Beispiele? Weil es bei dem reparieren von kaputten Fenstern genau das gleiche ist.

Am besten markiert man ein kaputtes fenster, das man nicht sofort fixen kann, mit einem todo kommentar.
Also ein "Achtung" schild, das anzeigt dass es bald gefixt wird. Ein Schild hinzustellen, das darauf aufmerksam macht, dass hier etwas nicht mit rechten dingen zugeht ist besser als gar nichts zu machen und es zu ignorieren. Fehler Klar zu benennen, anzusprechen und zu markieren hilft immer! ABER: Solche Todo kommentare sind nur erlaubt, wenn folgendes zutrifft:
1. es wurde bereits ein Jira-Ticket erstellt, in dem das Problem genau beschrieben wurde.
2. Im ToDo Kommentar steht die Ticketnummer dieses Tickets z.B. // ToDo: this bug will be fixed in Jira Ticket AB-12345.
3. Diesem Jira-Ticket wurde bereits ein dezidierter Software Engineer zugewiesen, der sich dafür verantwortlich fühlt, diesen Bug zu fixen.




-------------------------------------

## Wie zerbrochene Fenster fixen?

Eigene Notizen:

1. Wenn man ein nuees Feature im Code einbaut, sollte man "on the fly" bewusst nach einem zerbrochenem Fenster suchen und es fixen. Man sollte sich denken "was kann ich in diesem Projekt finden, das ich verbessern kann?". Fällt mir ein zerbrochenes Fenster schon während dem umsetzen des neuen Features auf? Umso besser - dann kann man es gleich fixen! Wenn man in seinem eigenen Haus ein neues Zimmer dazubaut und einem dabei ein zerbrochenes Fenster auffällt - warum dann nicht auch gleich das Fenster fixen? Das Werkzeug hat man schon heraußen - dann geht es gleich in einem. Jetzt ist man schon im Code "drinnen" und muss sich nicht mehr zusätzlich einlesen. Später ist es wieder schwieriger in die Marterie reinzukommen...

Das auch mit dem Atomic habits buch argumentieren:
Auch das Diagramm von S. 29 vom buch atomic habits einbauen. Da kann ich dann sagen: Bei Software ist es nochmal drastischer weil man - im vergleich zu menschen - davon ausgehen kann dass man wenn man software gar nicht anrührt, sie schon von haus aus jeden Tag schlechter wird und altert. Wenn menschen nix tun, dann bleiben die Ergebnisse auf der gestrichelten Linie. Aber wenn man bei Software nix tut, gibt es einen Rückgang von X%. Außerdem tendiert software von natur aus chaotisch zu werden wie wir im Kapitel "Entropie" besprochen haben. Deswegen ist es umso wichtiger, Software regelmäßig zu pflegen, bewusst anzugreifen und zu verbessern.

Es ist viel besser jeden Tag um 1% besser zu werden anstatt alles auf einmal machen zu wollen. Es ist viel besser die Applikation jeden Tag, konsequent, stetig mit jedem task, mit vielen kleinen einzelnen verbesserungen besser zu machen als zu denken, die gesamte applikation auf einmal in einem großen Task verbessern zu müssen. Hie und da mal ein Fenster fixen, den Müll auf der Straße loswerden, etc. Und die Psychologie/die Kultur/das Umfeld wird gleich besser und die Applikation wird besser! Es gibt kein "Perfekt" deswegen ist ein cycle an stetigem refactorn, verbessern, updaten und software am laufenden und aktuellen stand halten so notwendig um gute software zu entwickeln!

Ach ein weiterr Vorteil davon wenn man hin- und wieder mal kleine zerbrochene fenster on the fly fixt: man lernt kleine Ausschnitte von Applikationen immer besser kennen und man weiß dann, was sie machen.

2. Wenn einem ein zerbrochenes Fenster auffällt, dann SOFORT fixen!

3. Wenn einem ein zerbrochenes Fenster auffällt und man hat gerade keine Zeit/Möglichkeit es zu fixen, dann gleich ein ToDo Kommentar mit jira ticket und vernatwortlichen anlegen. (Hier die CPR-Rettung geschichte von vorher erzählen)




-----------------------------------------------