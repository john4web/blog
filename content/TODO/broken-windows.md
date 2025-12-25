+++
title = "The Broken Windows Theory in Software Engineering"
date = "2024-01-24"
description = ""
+++

{{< figure src="/images/broken-windows/broken_windows.jpg" width="80%" caption="Foto by [Tim Arterbury](https://unsplash.com/de/@tim_arterbury?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/de/fotos/zwei-zerbrochene-glasfenster-5Uh-wTSz-q0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)">}}


When I read the book *[The Pragmatic Programmer](https://en.wikipedia.org/wiki/The_Pragmatic_Programmer)*, one particular image wouldn’t leave my mind: _broken window panes_. In the chapter “Software Entropy”, Andrew Hunt and David Thomas describe a phenomenon that also applies to software engineering — the famous “broken windows theory”.  

In this blog post, I will explain what this theory is all about and how it can help protect software from slowly decaying over time.

## Software Entropy

The word "Entropy" originates from physics and describes the level of disorder within a system. According to the laws of thermodynamics, this disorder naturally tends to increase over time. 

{{< figure src="/images/broken-windows/entropy.jpg" width="40%" caption="The entropy of a closed system always increases. The system constantly tends toward maximum disorder. Just like the disorder in software.">}}

Entropy also manifests itself in software development. Just as in physical systems, disorder naturally grows over time, gradually causing software to decay. This is commonly known as “software rot”.  

Many factors can lead to software rot, but the most crucial one appears to be the psychology/mindset/culture, shaping the project. No matter how well a project is planned or how skilled the team, it can still fall into decay over time. Still, there are projects that, even in the face of major obstacles and persistent setbacks, succeed in defying the natural pull toward disorder and come out strong. But how?

## Broken Windows

In city neighborhoods, some buildings stay well-maintained and inviting, while others slowly crumble and fall apart. What causes this difference? Studies on urban decay and crime have identified a surprisingly small but powerful trigger that can turn a perfectly fine building into a neglected, abandoned structure in no time. The culprit? **A single broken window**.

Even one broken window, if left unfixed for a while, sends a powerful message to the building’s occupants: _"No one cares about this place!"_.
That small signal often sets off a chain reaction: more windows get damaged, trash starts piling up, graffiti appears, and structural problems begin to develop. In a surprisingly short time, the building can deteriorate to the point where repair seems pointless, and neglect becomes a reality.

The same thing applys to software! If you leave “broken windows” (bad designs, poor decisions, or sloppy code) unrepaired, the software will rot in no time! Software-Developers then think: _"If this code is already written bad, I might as well do it the same way. It doesn’t matter — no one cares anyway.”_

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

Das Haus symbolisiert saubere, gut gepflegte Software. Alles ist durchdacht, ordentlich und wertvoll – es lohnt sich, es zu erhalten.

Wortwörtlich: Ein Wandteppich fängt Feuer, die Feuerwehr kommt.
Metapher für Software: Ein plötzliches Problem oder eine Krise tritt auf (z. B. ein Bug, ein kritischer Fehler oder eine knappe Deadline).

Wortwörtlich: Die Feuerwehr schützt zuerst den teuren Teppich, bevor sie das Feuer löscht.
Metapher: Die Leute kümmern sich mehr um die „Ästhetik“ oder Ordnung, als um die unmittelbare Problemlösung.
Übertragung auf Software: Entwickler und Teams achten oft darauf, dass bestehender, sauberer Code nicht beschädigt wird, auch wenn sie unter Druck arbeiten. Sie versuchen, das „Haus“ sauber zu halten, während sie das „Feuer“ bekämpfen.
Gute Software motiviert zu sorgfältigem Arbeiten, selbst in Krisen.
Bedeutung: Ein kleiner Fehler oder eine schlechte Entscheidung im Projekt kann die gesamte Codebasis gefährden.

„Broken window“: Wie in der berühmten Broken-Window-Theorie: Ein einzelnes zerbrochenes Fenster signalisiert Vernachlässigung – und führt zu weiteren Schäden.

Kernaussage:

Schlechte Software zieht schlechte Software nach sich.

Guter, sauberer Code motiviert zu noch besserem Arbeiten.

Frühzeitiges Aufräumen und Pflegen ist entscheidend, um langfristigen Verfall zu verhindern.

Merksatz-Metapher:

„Ein Feuer löscht man besser, ohne das Haus zu zerstören – und saubere Software inspiriert die Entwickler, sorgfältig zu bleiben.“

## Refutation of the counterarguments

- keiner hat die zeit ständig kaputte fenster in projekten zu fixen
Meinung dahinter: XXXX

Realität: Aber hat man später dann die Zeit für den Mehraufwand wenn man neue Features einbauen will und man braucht ewig dafür?

- keiner hat bock ständig kaputte fenster in projekten zu fixen
Meinung dahinter: XXX

Realität: Aber das ist genau deine Arbeit als Software Engineer. Lebe damit! Außerdem kostet dich das sicher weniger Mühe, als später eine komplett verrottete Applikation warten zu müssen.

- es rentiert sich nicht das anzugreifen: das muss irgendwann mal sowieso von grund auf neu geschrieben werden. "Das Haus nicht sanieren sondern gleich neu bauen".
Meinung dahinter: XXXXXX

Realität: XXXXX

- „Es funktioniert ja sowieso noch.“
Meinung dahinter: Solange der Fehler die Funktionalität nicht direkt beeinträchtigt, ist es Zeitverschwendung, ihn jetzt zu beheben.

Realität: Auch kleine Probleme können sich summieren und später schwerwiegende Fehler verursachen („Software-Rot“).

- „Ich habe gerade keine Zeit dafür.“
Meinung dahinter: Andere Aufgaben oder Deadlines haben Vorrang.

Realität: Aufgeschobene kleine Fehler werden oft später teurer und aufwendiger zu beheben.

- „Das stört doch niemanden.“
Meinung dahinter: Fehler in weniger genutzten Modulen oder Bereichen sind irrelevant.

Realität: Über Zeit beeinflussen solche „kleinen Fenster“ die Codequalität, erschweren Wartung und erhöhen das Risiko von Bugs.

- „Ich will den Code nicht jetzt anfassen, ich plane größere Refactorings.“
Meinung dahinter: Beheben würde später sowieso wieder überschrieben werden.

Realität: Kleine Verbesserungen stören selten größere Umbauten und verhindern, dass sich schlechte Gewohnheiten festsetzen.

- „Es ist nicht mein Problem.“
Meinung dahinter: Fehler liegen außerhalb des eigenen Verantwortungsbereichs.

Realität: Softwareprojekte leben von gemeinsamer Verantwortung; Ignorierte Fehler belasten das Team langfristig.


## How to Fix Broken Windows?
Alright! We are now clear that broken windows must be repaired without objection or doubt. But what is the best way to do this? In the following chapter, I will discuss what to keep in mind when fixing broken windows and how to go about it.

### Later is the Day that never comes

Welche Regeln sollte man beim fixen von zerbrochenen Fenstern beachten? Nun, hierbei verhält es sich ähnlich wie bei FightClub (ich hoffe ihr habt den Film gesehen):

Die erste Regel beim fixen von zerbrochenen Fenstern lautet:

> Zerbrochene Fenster gehören immer sofort gefixt!

Die zweite Regel beim fixen von zerbrochenen Fenstern lautet:

> Zerbrochene Fenster gehören immer sofort gefixt!

Warum ist das so wichtig? Nun - ein Bild sagt mehr als tausend Worte:

{{< figure src="/images/broken-windows/later.png" caption="Fixing stuff later in Software Engineering (Source: [Twitter Post from Vlad Mihalcea](https://x.com/vlad_mihalcea/status/1802901724362887580))." width="400">}}

> In software development, later is the day that never comes.

Es ist immer dasselbe: wenn man in der Software-Entwicklung etwas nicht sofort macht, dann macht man es nie.

Ich habe das schon so oft in der Arbeit erlebt:
"Das machen wir später, wenn wir besser Zeit dafür haben!"
ist ein Synonym für
"Dieses Problem ist gerade unangenehm und schwierig zu lösen. Wir machen es nie!".

Man geht davon aus, dass das Problem in Zukunft leichter zu lösen ist als jetzt. Aber das ist oft ein Trugschluss. Man weiß gar nicht ob es in Zukunft einen besseren Zeitpunkt dafür gibt. Aus Erfahrung weiß ich: in Zukunft wird das Problem immer schwieriger werden. Wie die Entropie besagt: Chaos nimmt von natur aus zu. Alles wird immer komplizierter, wenn sich software weiterentwickelt. Wenn über das Hütchen bereits drüberzementiert wurde (Siehe Bild).

Warum also nicht gleich jetzt fixen? Auf später verschieben bringt nichts, denn es wird nie den perfekten Zeitpunkt geben um das Problem zu fixen. "The best time to start is now!"

Ich weiß - in der Praxis ist es oft nicht so leicht Sachen sofort zu fixen. Und oft kommen einem wirklich Sachen dazwischen und etwas anderes ist gerade wichtiger zu machen. z.B. Critical Bugs in production. Wenn man wirklich einen triftigen Grund hat, das Fenster gerade nicht zu fixen, hat man immer noch die Möglichkeit ein "Achtung-Schild" aufzustellen!

### Das Achtung Schild

Vor kurzem war ich am wiener Westbahnhof unterwegs und habe etwas gesehen, das mich sehr an dieses ganz Thema erinnert hat. Auf dem Weg zur Ubahn sah ich direkt vor einer Starbucks-Filiale folgendes:

{{< figure src="/images/broken-windows/sign.jpg" caption="Ein 'Achtung-Rutschgefahr' Schild auf einer Pfütze verschüttetem Kaffee. Der Kaffe scheint schon etwas eingetrocknet zu sein. Das Schild dürfte also schon eine Weile dort stehen. 😅" width="400">}}

Da hat scheinbar jemand seinen Kaffee verschüttet. Und ein Mitarbeiter:in hat das scheinbar gesehen. Anstatt es aber gleich wegzuputzen, hat der mitarbeiter einfach dieses "Achtung Rutschgefahr" Schild draufgestellt. Vielleicht hat der Mitarbeiter gerade etwas anderes zu tun gehabt und hatte keine Zeit es gleich wegzuputzen. Vielleicht musste er kunden bedienen. Vielleicht hat der Mitarbeiter sich aber auch gedacht "Der Kaffee wurde nicht am Boden der Starbucks-Filiale verschüttet sondern am Bahnhofsboden - ich bin nicht zuständig dafür. Da sollen sich gefälligst die Bahnhofs-Putzfrauen drum kümmern!". Was auch immer der Grund war - wir werden es nie erfahren. Aber eines ist schonmal klar: Es ist gut, dass die Person das Schild überhaupt aufgestellt hat! Viel besser wäre es natürlich gewesen, wenn er den Fleck ohne zu zögern gleich weggeputzt hätte. Aber es wäre viel viel Viel schlimmer gewesen, wenn der Mitarbeiter gesehen hätte wie der Kaffee ausgeschüttet wird, es aber einfach ignoriert hätte. Also es ist schonmal gut dass er überhaupt reagiert hat. Fehler Klar zu benennen, anzusprechen und zu markieren hilft immer!

Das Schild bedeutet: Aufpassen! Gebt Acht! Hier ist ganz klar etwas schief gelaufen! Das Problem gehört schnellstmöglich behoben! 

Umgemünzt auf die Software-Entwicklung ist es genauso! Wenn man ein zerbrochenes Fenster, einen Kaffeefleck, einen Programmfehler oder wie man es auch immer nennen möchte sieht, sollte man es sofort fixen. Wenn man aber keine Zeit hat es zu fixen, muss man zumindest sofort reagieren und ein "Achtung-Schild" aufstellen. Nur ist es sehr wichtig was auf diesem Schild draufsteht (Message is Key)! Aber was sollte man da drauf schreiben? Der nächste Abschnitt versucht das zu erklären:

### CPR und Rettungssanitäter

Als ich  meine Oberstufe abgeschlossen hatte, habe ich 9 Monate beim Österreichischen roten Kreuz als Rettungssanitäter verbracht. Dort habe ich gelernt, wie man erste Hilfe leistet und Personen in akuten Notsituationen rettet. Bei der dazugehörigen Rettungssanitäter-Ausbildung ist mir ein Kommentar von einem Ausbildner im Kopf hängen geblieben.

Stellen sie sich folgendes Szenario vor: 
Wenn jemand auf der straße zusammenbricht, gehen alle leute vorbei und keiner fühlt sich zustänndig. Jeder der vorbeigeht denkt sich "das wird schon jemand anderer sehen bzw. da wird schon jemand anderer dafür verantwortlich sein diesem menschen zu helfen". Aber weil sich das jeder denkt, wird ihm niemand helfen. Wenn alle verantwortlich sind, dann fühlt sich wiederrum gar keiner verantwortlich.

Dasselbe ist in folgendem szenario: stell dir vor jemand bricht zusammen und hat einen herz-kreislauf stillstand. Dann geht jemand hin und macht CPR. Ebendiese Person screit dann "ruft die rettung"! Das ist das schlechteste was man machen kann. Wenn man alle rundherum gleichermaßen anspricht, dann fühlt sich wieder keine person zuständig und jeder denkt sich "wird schon jemand anderer machen". Eine bessere Methode wäre, eine dezidierte Person, welche in der Nähe rumsteht auszuwählen und sie direkt ansprechen. Mit dem Finger auf sie zeigen! Und dann rufen "Hey du! Du rufst jetzt die Rettung und sagst dem Disponent dass es sich um einen Herz-Kreislauf Stillstand handelt!". Dadurch fühlt sich eine Person direkt angesprochen und wird viel wahrscheinlicher handeln. Wichtig: man muss die angesprochene Person dazu bringen sich verantwortlich zu fühlen! Sonst passiert nichts. 

Warum bringe ich jetzt diese Beispiele? Weil es bei dem reparieren von kaputten Fenstern genau das gleiche ist. Wenn man ein "Achtung-Schild" aufsteht, darf es nicht untergehen oder ignoriert werden. Die verantwortlichen Personen müssen klar definiert sein.


wie wir in dem obigen Twitter Post von Vlad Mihalcea bereits gesehen haben, ist folgendes ein schlechtes Beispiel für ein "Achtung-Schild":


// ToDo: Fix later
    boolean isValid = false;
    if(isValid == true){
        System.out.println("do something");
    }


Es ist viel zu ungenau, man sieht nicht gleich auf den ersten Blick was das Problem ist, es wurde kein Bezug und keine dringlichkeit geschaffen. Das Kommentar ist genauso schlampig wie der Code selber.


Gute Message:

/*
ToDo: Remove unnecessary boolean comparison
Comment by: John Doe (j.d@example.com)
Noticed on: 2025-01-13
Reason: isValid == true is redundant
Ticket: ABC-215 https://jira.at/browse/ABC-215
Person in charge: Johannes Gerstbauer (j.g@example.com)
*/
    boolean isValid = false;
    if(isValid == true){
        System.out.println("do something");
    }

Wie sieht dieses "Achtung-Schild" hingegen aus?
1. ToDo: Remove unnecessary boolean comparison
Hier wird erklärt, dass es einen Handlungsbedarf und kurz beschrieben was hhier nicht mit rechten dingen zugeht.

2. Comment by: John Doe (j.d@example.com)
Hier hat sich diejenige Person verewigt, die das "Achtung-Schild" aufgestellt hat. Wichtig um evtl. nochmal nachfragen zu können was sich die Person dabei gedahct hat.

3. Noticed on: 2025-01-13
Hier wurde das Datum hinzugefügt, wann das "Achtung-Schild" aufgestellt wurde. Wichtig für die Nachverfolgung. Un hoffentlich bekommt man auch dann ein schlechtes Gewissen, wenn das Datum recht weit zurückliegt und das zerbrochene Fenster noch immer nicht gefixt wurde.

4. Reason: isValid == true is redundant
Hier kann das zugrundeliegende Problem nochmal etwas genauer beschrieben werden.

5. Ticket: ABC-215 https://jira.at/browse/ABC-215
Dieser Schritt ist ganz wichtig! Damit auch Produktverantwortliche bescheid wissen, sollte ein JIRA-Bug-Ticket für den Task erstellt werden. In dem Jira-Ticket sollte im Description Feld möglichst detailliert beschrieben werden, was das Problem ist. Dem Ticket sollte dann auch gleich in Jira ein Software-Engineer zugewiesen werden, der das Ticket umsetzt. Dann kann evtl. der Task in Scrum-Sprints eingeplant werden. Außerdem ist dann das Problem viel greifbarer, nachvollziehbarer und dokumentiert. Es wird im Team aktiv darüber gesprochen und diskutiert. Und dadurch ist auch die Wahrscheinlichkeit geringer, dass das Thema vergessen wird.

6. Person in charge: Johannes Gerstbauer (j.g@example.com)
Dies ist nun das Rettungs-Thema: Es muss klar definiert werden, wer das zerbrochene Fenster fixt. Wenn man das nicht macht, fühlt sich niemand verantwortlich und das Thema wird vergessen. Es muss nicht immer automatisch jemand anderer bestimmt werden. Es kann auch gleich im vorhinein bestimmt werden, dass die Person, die das Schild aufstellt, später auch das Problem löst. Das ist in den meisten Fällen sogar am besten weil die Person, die das Schild aufstellt, auch gleich am besten weiß was das Problem ist.

Jetzt hab ich euch gezeigt wie man am besten solche ToDo: Kommentare hinterlässt. Ich hoffe dieser Guideline hilft euch!

Jeder andere entwickler, der nun dieses Achtung-Schild sieht, kann dann bei der "Person in charge" nachfragen - "wurde das schon gefixt"? "Warum wurde das noch nicht gefixt"? "Was ist der Plan"? "Wann wird das gefixt"? Wird das noch gefixt? "Ist das Thema untergegangen"?

So werden auch keine zerbrochenen Fenster mehr vergessen.

### Die 1%-Regel

Wenn man ein nuees Feature im Code einbaut, sollte man "on the fly" bewusst nach einem zerbrochenem Fenster suchen und es fixen. Man sollte sich denken "was kann ich in diesem Projekt finden, das ich verbessern kann?". Fällt mir ein zerbrochenes Fenster schon während dem umsetzen des neuen Features auf? Umso besser - dann kann man es gleich fixen! Wenn man in seinem eigenen Haus ein neues Zimmer dazubaut und einem dabei ein zerbrochenes Fenster auffällt - warum dann nicht auch gleich das Fenster fixen? Das Werkzeug hat man schon heraußen - dann geht es gleich in einem. Jetzt ist man schon im Code "drinnen" und muss sich nicht mehr extra reindenken. Später ist es wieder schwieriger sich einzulesen/reinzudenken, in die Marterie reinzukommen...

Das auch mit dem Atomic habits buch argumentieren:
Auch das Diagramm von S. 29 vom buch atomic habits einbauen. Da kann ich dann sagen: Bei Software ist es nochmal drastischer weil man - im vergleich zu menschen - davon ausgehen kann dass man wenn man software gar nicht anrührt, sie schon von haus aus jeden Tag schlechter wird und altert. Wenn menschen nix tun, dann bleiben die Ergebnisse auf der gestrichelten Linie. Aber wenn man bei Software nix tut, gibt es einen Rückgang von X%. Außerdem tendiert software von natur aus chaotisch zu werden wie wir im Kapitel "Entropie" besprochen haben. Deswegen ist es umso wichtiger, Software regelmäßig zu pflegen, bewusst anzugreifen und zu verbessern.

Es ist viel besser jeden Tag um 1% besser zu werden anstatt alles auf einmal machen zu wollen. Es ist viel besser die Applikation jeden Tag, konsequent, stetig mit jedem task, mit vielen kleinen einzelnen verbesserungen besser zu machen als zu denken, die gesamte applikation auf einmal in einem großen Task verbessern zu müssen. Hie und da mal ein Fenster fixen, den Müll auf der Straße loswerden, etc. Und die Psychologie/die Kultur/das Umfeld wird gleich besser und die Applikation wird besser! Es gibt kein "Perfekt" deswegen ist ein cycle an stetigem refactorn, verbessern, updaten und software am laufenden und aktuellen stand halten so notwendig um gute software zu entwickeln!

Ach ein weiterr Vorteil davon wenn man hin- und wieder mal kleine zerbrochene fenster on the fly fixt: man lernt kleine Ausschnitte von Applikationen immer besser kennen und man weiß dann, was sie machen. So wird man zu einem besseren und wertvollerem Software Engineer.

----------------------------------------------------------------------

Hier auch über "boiled frogs" vom Buch "Pragmatic Programmer" schreiben.
(S. 4 bis 9) und (S. 224 bis 225)

-------------------------------------

## Fazit

Schlechter, unordentlicher Code oder schlechte Entscheidungen wirken wie ein „zerbrochenes Fenster“.

Wenn man kleine Probleme ignoriert, verschlechtert sich das gesamte Projekt schnell – jeder im Team denkt „Wenn das hier schon schlecht ist, kann ich es auch so machen“. Menschen passen sich dem Umfeld an. 

Umgekehrt motiviert sauberer, eleganter Code das Team, sorgfältig zu arbeiten, selbst unter Druck (Deadlines, Releases etc.).

Kernidee: Qualität zieht Qualität an, Schlampigkeit zieht Schlampigkeit an.

vorgehen beim fixen von zerbrochenen Fenstern:

1. sofort fixen
2. wenn man gerade keine Zeit hat, dann ein "Achtung-Schild" aufstellen und eine verantwortliche Person klar definieren.
3. Man sollte bei jedem neuen Feature gezielt nach zerbrochenen Fenstern suchen und diese fixen. Pro neuem Feature 1 zerbrochenes Fenster!

2. Wenn einem ein zerbrochenes Fenster auffällt, dann SOFORT fixen!

3. Wenn einem ein zerbrochenes Fenster auffällt und man hat gerade keine Zeit/Möglichkeit es zu fixen, dann gleich ein ToDo Kommentar mit jira ticket und vernatwortlichen anlegen. (Hier die CPR-Rettung geschichte von vorher erzählen)


## Reference

- Atomic Habits Buch
- Pragmatic Programmer Buch
- https://x.com/vlad_mihalcea/status/1802901724362887580