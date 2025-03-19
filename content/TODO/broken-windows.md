+++
title = "The Broken Windows Theory in Software Engineering"
date = "2024-01-24"
description = ""
+++

Wird im Buch "Pragmatic Programmer" erklärt. 



In software development, later is the day that never comes.

{{< figure src="/images/broken-windows/later.jpeg" caption="Fixing stuff later in Software Engineering (Source: [Twitter Post from Vlad Mihalcea](https://x.com/vlad_mihalcea/status/1802901724362887580))." width="400">}}

Hier auch über "boiled frogs" vom Buch "Pragmatic Programmer" schreiben.
(S. 4 bis 9) und (S. 224 bis 225)

Auch drüber schreiben, dass man "Fenster", die man nicht sofort fixen kann, auch mit einem TODO kommentar versehen kann. Also ein "Achtung" schild markiert, das anzeigt dass es bald gefixt wird. Aber: nur todo kommentare mit Ticketnummern sind erlaubt. 

Hier die analogie einfügen mit der Rettung:

Wenn jemand auf der straße zusammenbricht, gehen alle leute vorbei und keiner fühlt sich zustänndig. Jeder der vorbeigeht denkt sich "das wird schon jemand anderer sehen bzw. da wird schon jemand anderer dafür verantwortlich sein diesem menschen zu helfen". Aber weil sich das jeder denkt, wird ihm niemand helfen.

Dasselbe ist in folgendem szenario: stell dir vor jemand bricht zusammen und hat einen herz-kreislauf stillstand. Dann geht jemand hin und macht CPR. Ebendiese Person screit dann "ruft die rettung"! Das ist das schlechteste was man machen kann. Wenn man alle rundherum gleichermaßen anspricht, dann fühlt sich wieder keine person zuständig und jeder denkt sich "wird schon jemand anderer machen". Eine bessere Methode wäre, eine dezidierte Person, welche in der Nähe rumsteht auszuwählen und sie direkt ansprechen. "Hey du! Du rufst jetzt die Rettung und sagst dem Disponent dass es sich um einen Herz-Kreislauf Stillstand handelt!". Dadurch fühlt sich eine Person direkt angesprochen und wird viel wahrscheinlicher handeln. Warum bringe ich jetzt diese Beispiele? Weil es bei dem reparieren von kaputten Fenstern genau das gleiche ist.

Am besten markiert man ein kaputtes fenster, das man nicht sofort fixen kann, mit einem todo kommentar.
Also ein "Achtung" schild, das anzeigt dass es bald gefixt wird. Aber: Solche Todo kommentare sind nur erlaubt, wenn folgendes zutrifft:
1. es wurde bereits ein Jira-Ticket erstellt, in dem das Problem genau beschrieben wurde.
2. Im ToDo Kommentar steht die Ticketnummer dieses Tickets z.B. // ToDo: this bug will be fixed in Jira Ticket AB-12345.
3. Diesem Jira-Ticket wurde bereits ein dezidierter Software Engineer zugewiesen, der sich dafür verantwortlich fühlt, diesen Bug zu fixen.
