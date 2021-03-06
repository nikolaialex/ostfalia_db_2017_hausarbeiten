# Grundlagen
## ACID-Prinzip [1]
Das ACID-Prinzip beschreibt erwünschte Eigenschaften von transaktionsbasierten Datenbanken sowie von verteilten Systemen. Die Bezeichnung ist eine Abkürzung und steht für folgenden Eigenschaften:

### Atomicity
Alle Datanbankoperationen in einer Transaktion sind ununterbrechbar angelegt und werden zusammen ausgeführt. Schlägt eine Einzeloperation fehl, schlägt die gesamte Transaktion fehl (Alles-oder-nichts-Eigenschaft).

### Consistency
Die Konsistenz der Datenbank muss mittels Integritätserhaltung gewährleistet werden.

### Isolation
Durch den isolierten Ablauf von Transaktionen wird erreicht, dass parallel ablaufende Transaktionen sich nicht gegenseitig beeinflussen können. Erreicht wird dies durch Serialisierbarkeit.

### Durability
Alle Operationen haben eine persistente Wirkung.

## CAP-Theorem
Das CAP-Theorem wurden von Eric A. Brewer aufgestellt und besagt, dass maximal zwei der drei Eigenschaften "Consistency" (Konsistenz), "Availability" (Verfügbarkeit) und "Partition tolerance" (Partitionstoleranz) bei einem verteilten System garantiert werden können. Eine Konzentration auf eine dieser Eigenschaften hat somit nachteiligen Effekt auf die anderen Eigenschaften.

## BASE-Prinzip [2]
Autor des BASE-Prinzips ist Eric A. Brewer, welcher ebenfalls das CAP-Theorem aufstellte, wonach nur entweder eine vollständige Erreichbarkeit oder eine vollständige Konsistenz einer Datenbank ermöglicht werden kann. Somit wurde es notwendig, bei verteilten Datenbanken einen Weg zu beschreiben, bei denen die Daten immer verfügbar und nahezu konsistent sind. Eine Möglichkeit besteht nach Brewer darin, dass Server Änderungen der Daten asynchron erhalten und somit immer lesbar bleiben und nur für eine kurze Zeit inkonsistent sind.

Diese Erkenntnis manifestiert Brewer im BASE-Prinzip, welches eine Abkürzung für die folgenden Punkte ist:

** Basically Available**: Grundsätzliche Verfügbarkeit

** Soft State**: Die Datenmenge wird periodisch abgespeichert

** Eventual Consistency**: Die Konsistenz wird irgendwann erreicht. asynchrone Speicherung



## Zeitreihe
Eine Zeitreihe, ursprünglich aus der statistischen Literatur kommend, ist eine Reihe von Beobachtungen zu verschiedenen Zeitpunkten (z.B. eine ökonomische Zeitreihe, eine Wetterzeitreihe). Ab der Mitte der 1920er Jahre bezeichnet der Begriff zumeist einen stochastischen Prozess, der in der Praxis durch Beobachtungen realisiert wird. Das Gebiet, welches mit diesen Zeitreihen arbeitet, wird als Zeitreihenanalyse bezeichnet. Unter Zeitreihenanalyse versteht man die statistische Analyse stochastischer Prozesse. [3]


## Echtzeitsystem
Unter einem Echtzeitsystem werden Systeme verstanden, die auf Impulse (Interrupts) innerhalb einer festgelegten Zeit angemessen reagieren. So kann erreicht werden, dass diese Systeme immer vorhersehbar schnell antworten. Dies ist z.B. bei integrierten Schaltungen in einem CD Player notwendig. Die Daten werden durch das Laufwerk gelesen und an den Prozessor geschickt. Dieser muss nun die Daten alle in einer sehr kurzen Zeit dekodieren und ausgeben. Dauert dies zu lange, würde sich die Musik komisch anhören.
Weitere Echtzeitanwendungen wären unter anderem Autopiloten, Robotersteuerungen in Fabriken, Medizingeräte oder auch allerlei Gerät beim Militär.
Im Falle einer verspäteten Antwort, wäre dies in solchen Einsatzgebieten genauso schlecht wir keine Antwort.

Allgemein werden Echtzeitsysteme unterteilt in harte Echtzeitsystem, bei welchen es absolute Grenzen gibt und weiche Echtzeitsysteme, bei welchen eine Grenzüberschreitung zwar unerwünscht ist aber toleriert wird. [4]


[Inhaltsverzeichnis](02_toc.md) | [Einleitung](03_2_einleitung.md)
