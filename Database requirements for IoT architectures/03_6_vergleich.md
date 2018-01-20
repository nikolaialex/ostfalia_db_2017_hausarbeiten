# Vergleich relationales zu schemafreies Datenbankmanagementsystem

Im Folgenden werden die aus dem vorherigem Kapitel genannten Anforderungen für die relationale und schemafreie Datenbanken überprüft, um zu schauen, ob es neue Datenbanksysteme für die IoT-Architektur braucht, oder ob bereits die heutigen System in der Lage sind, dies Aufgaben zu erfüllen.

## relationales Datenbanksystem
### Skalierbarkeit
Relationale Datenbanken skalieren in der Regel vertikal, also auf dem Host. Für die horizontale Skalierung über mehrere Hosts sind relationale DBMS nur bedingt geeignet, da durch die Transaktionssicherheit auf allen Hosts die Transaktionen durchgeführt werden müssen und so die Schreib- und Leseprerformance negativ beeinträchtigen.

### Echtzeitverarbeitung
Relationale Datenbanken arbeiten mit strukturierten Daten und verbinden diese mittels JOINs zu großen Datenmengen. Dieses Konzept sorgt in der Praxis dafür, dass die Datenbank für vielen Anfragen neue temporäre Tabellen erstellen und auswerten muss. All diese Operationen benötigen Zeit bei der Ausführung und sorgen so dafür, dass für Echtzeit-Anwendungen relationale Datenbanken nur bedingt geeignet sind, da die Ausführungszeit von Anfragen nicht vorherbestimmt ist.

### heterogene Nutzdaten
Durch die tabellenartige Speicherung der Daten in einem relationalem DBMS werden die Daten nicht als ein Block gespeichert, sondern in Spalten aufgeteilt. Stehen einige dieser Daten in Relation, werden diese nach den Regeln der Normalisierung auf mehrere Tabellen aufgeteilt. Dies ist ein sehr gutes Prinzip, wenn die Datenmengen zuvor bekannt sind und nicht oft erweitert werden müssen. Bei der Nutzung in einem IoT-Umfeld wird aber gerade dies oft vorkommen, da neue IoT-Geräte mit neuen Sensoren neue Daten senden, die dann in den Tabellen eingepflegt werden müssen. Natürlich kann das Design der Tabellen dies begünstige, so dass z.B. die Nutzdaten in einer Spalte als JSON abgelegt werden. So ist der Nachteil der Erweiterbarkeit dem der Strukturiertheit gewichen.

### Transaktionssicherheit
Relationale Datenbank versuchen das ACID-Prinzip umzusetzen und bündeln so die Operationen in Transaktionen. Mittels verschiedener LOCK-Arten ist es zudem möglich den Lese-/Schreibzugriff auf Tabellen während dieser Transaktionen zu steuern. Ebenfalls werden mittels Commit-Protokollen Transaktionen auf mehreren replizierten Hosts ausgeführt, so dass die Transaktion auf allen Hosts sichergestellt ist. Somit kommt es zu Verzögerungen der Ausführung. Im Hinblick auf die IoT-Daten sind Transaktionen nicht zu streng zu sehen. Vielmehr ist es wichtig, dass die Daten möglichst schnell zur Verfügung stehen und abgefragt werden können.

### Hohe Verfügbarkeit
Hohe Verfügbarkeit kann bei relationalen Datenbanken mittels eines Clusters aus Servern entstehen. Hierbei wird ein Master in dem Verbund auf Slaves repliziert. Werden Daten im Master verändert, werden diese innerhalb einer Transaktion auch auf den Slaves angepasst. Zu Beachten ist hier das CAP-Theorem, welches besagt, dass maximal zwei der drei Eigenschaften Konsistenz, Verfügbarkeit und Ausfalltoleranz  erreichbar sind. Somit geht die Realisierung einer hohen Verfügbarkeit zu Lasten der Konsistent oder der Verfügbarkeit. Das CAP-Theorem meint die Verfügbarkeit im Sinne akzeptabler Antwortzeiten.

### Räumlich-zeitliche Skalierbarkeit
Die Fähigkeit die Daten strukturiert in einer Tabelle abzulegen bietet auch die Möglichkeit Relationen zu den Daten hinzuzufügen. In der relationalen Datenbank würden somit z.B. der Standort über eine Relation den einzelnen Messwerten bzw. den Geräten hinzugefügt werden. mittels JOIN-Abfragen können diese wieder in eine Struktur gebracht werden und so sehr detailliert abgefragt werden.

### Schnell und zuverlässig
Die Geschwindigkeit der Beantwortung von Abfragen hängt bei relationalen DBMS stark von deren Struktur und Komplexität ab. Während bei kleineren Datenbanken noch mit Indizies eine Beschleunigung erreicht werden kann, ist dies bei sehr großen Datenbanken schlecht realisierbar, wenn die Indizies nicht mehr komplett in den Hauptspeicher passen.
[14]

Ein Master/Slave Verbund kann zwar bei Leseoperationen einen Performancegewinn liefern, hat aber Schwächen bei der Geschwindigkeit von Schreiboperationen, welche bei mehreren tausenden IoT-Geräte zum Flaschenhals werden wird.

### Systemreife
Relationale DBMS sind bereits lange im Einsatz und besitzen somit einen guten Grad an Systemreife. Sie bieten umfangreiche Schutzmechanismen mit Authentifizierung und Authorisierung, teilweise sogar auf Tabellenspalten-Ebene.
[10]




## schemafreies Datenbanksystem
### Skalierbarkeit
Schemafreie Datenbanken können vertikal als auch horizontal skalieren. Die Besonderheit der nicht Befolgung des ACID-Prinzips, sondern des BASE-Prinzips, sorgt dafür, dass die horizontale Skalierung einfach möglich wird. Dies liegt an der Anforderung, dass die Datenbank immer erreichbar ist, aber die Konsistenz nicht zu jedem Zeitpunkt erfüllt sein muss. Somit lassen sich viele Hosts in einem Verbund bringen und eine Lastverteilung erreichen.

### Echtzeitverarbeitung
Durch die schemafreie Speicherung und das nicht verfolgen des ACID-Prinzips lassen sich bei großen Datenmengen Performancesteigerungen erzielen, welche bei Echtzeitanwendungen essentiell sind. Dies wird unter anderem durch die Art der möglichen Datenmodelle wie z.B. key-value oder graph erreicht.
[9][11]

### heterogene Nutzdaten
Durch die schemafreie Speicherung der Daten, liegt es in der Natur solche Systeme, dass diese sehr gut mit heterogenen Nutzdaten umgehen können. Die Daten werden in die Datenbank eingelesen und über gewisse Felder z.B. die Geräte-ID oder den Standort kann ein Index erzeugt werden. Aufwändige Erweiterungen der Datenbanken, wie bei relationalen DBMS entfallen, wenn neue Nutzdaten mit abweichendem Schema hinzu kommen.

### Transaktionssicherheit
In schemafreien Datenbanken ist aufgrund der Anwendung des BASE-Prinzip die Transaktionssicherheit nicht so gewährleistet, wie es bei relationalen Datenbanksystemen der Fall ist. Durch die asynchrone Art der Konsistenz können einzelnen Knoten im Cluster einen alten Stand der Daten wiedergeben, da diese noch nicht aktualisiert wurden. Dafür sind die Daten ständig verfügbar und können schnell gelesen werden.

### Hohe Verfügbarkeit
Durch horizontale Skalierung einer schemafreien Datenbank können viele Knoten in einem Cluster erzeugt werden, die eine hohe Verfügbar der Daten gewährleisten. Ein Ausfall eines Knoten kann über die weiteren Knoten kompensiert werden. Durch die Replikation nach dem BASE-Prinzip kann allerdings ein Knoten für eine kurze Zeit mit nicht aktuellen Daten existieren, welches bei Echzeit-Anwendungen zu beachten ist.

### Räumlich-zeitliche Skalierbarkeit
Eine räumliche und zeitliche Zuordnung von Messdaten kann in einer schemafreien Datenbank über unterschiedliche Methoden erfolgen. Die gängigste Methode ist das Tagging von Dokumenten. Hierbei werden die Messdaten dem Dokument, welches einem IoT-Gerät entspricht, angehängt und das Dokument mit Tags für z.B. den Standort versehen. Über die Tags können bei Abfragen schnell die betroffenen Dokumente identifiziert werden und die Zeitserie mit den Messdaten ausgelesen werden.

### Schnell und zuverlässig
In schemafreien Datenbanksystemen kann eine hohe Lesegeschwindigkeit bei heterogenen Daten erreicht werden, da diese nicht erst vereinheitlicht werden müssen. Im Gegensatz zu relationalen Datenbanken müssen die Informationen nicht aus verschiedenen Tabellen über Relationen aufgelöst werden und stehen somit schneller zur Verfügung. Bei Schreibzugriffen verhält es sich ähnlich, da durch das BASE-Prinzip nicht alle Knoten eines Cluster zeitgleich geändert werden müssen und die Konsistenz asynchron erreicht werden kann.

### Systemreife
Im Gegensatz zu relationalen Datenbanken bieten schemafreie nicht immer ein ausführliches Sicherheitskonzept z.B. auf Basis von Tabellenspalten. Das fehlen solcher Feature ist dabei aber der Performance dienlich. Sicherheitsfeature müssen somit dann über externe Maßnahmen implementiert werden.
[10]


[Anforderungen an Datenbanken](03_5_merkmale.md) | [Fazit](04_fazit.md)
