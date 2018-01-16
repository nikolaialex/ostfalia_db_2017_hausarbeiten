# Vergleich relationales zu schemafreies Datenbanksystem

Im Folgenden werden die aus dem vorherigem Kapitel genannten Anforderungen für die relationale und schemafreie Datenbanken überprüft, um zu schauen ob es neue Datenbanksysteme für die IoT-Architektur braucht, oder ob bereits die heutigen System in der Lage sind, dies Aufgaben zu erfüllen.

## relationales Datenbanksystem
### Skalierbarkeit
Relationale Datenbanken skalieren vertikal, also auf dem Host. Für die horizontale Skalierung über mehrere Hosts sind relationale DBMS nur bedingt geeignet, da durch die Transaktionssicherheit auf allen Hosts die Transaktionen durchgeführt werden müssen und so die Schreib- und Leseprerformance negativ beeinträchtigen.

### Echtzeitverarbeitung
Relationale Datenbanken arbeiten mit strukturierten Daten und verbinden diese mittels JOIN zu großen Datenmengen. Dieses Konzept sorgt in der Praxis dafür, dass die Datenbank für vielen Anfragen neue temporäre Tabellen erstellen und auswerten muss. All diese Operationen benötigen Zeit bei der Ausführung und sorgen so dafür, dass für Echtzeit-Anwendungen relationale Datenbanken nur bedingt geeignet sind, da die Ausführungszeit von Anfragen nicht vorherbestimmt ist.

### heterogene Nutzdaten
Durch die tabellenartige Speicherung der Daten in einem relationalem DBMS werden die Daten nicht als ein Block gespeichert, sondern in Spalten aufgeteilt. Stehen einige dieser Daten in Relation, werden diese nach den Regeln der Normalisierung auf mehrere Tabellen aufgeteilt. Dies ist ein sehr gutes Prinzip, wenn die Datenmengen zuvor bekannt sind und nicht oft erweitert werden müssen. Bei der Nutzung in einem IoT-Umfeld wird aber gerade dies oft vorkommen, da neue IoT-Geräte mit neuen Sensoren neue Daten senden, die dann in den Tabellen eingepflegt werden müssen. Natürlich kann das Design der Tabellen dies beeinflussen, so dass z.B. die Nutzdaten in einer Spalte als JSON abgelegt werden. So werden ist der Nachteil der Erweiterbarkeit dem der Strukturiertheit gewichen.

### Transaktionssicherheit
Relationale Datenbank versuchen das ACID-Prinzip umzusetzen und bündeln so die Operationen in Transaktionen. Mittels verschiedener LOCK-Arten ist es zudem möglich den Lese-/Schreibzugriff auf Tabellen während dieser Transaktionen zu steuern. Ebenfalls werden mittels Commit-Protokollen Transaktionen auf mehreren replizierten Hosts ausgeführt, so dass die Transaktion auf allen Hosts sichergestellt ist. Somit kommt es wieder zu Verzögerungen der Ausführung. Im Hinblick auf die IoT-Daten sind Transaktionen nicht zu streng zu sehen. Vielmehr ist es wichtig, dass die Daten möglichst schnell zur Verfügung stehen und abgefragt werden können.

### Hohe Verfügbarkeit
Hohe Verfügbarkeit kann bei relationalen Datenbanken mittels eines Clusters aus Servern entstehen. Hierbei wird ein Master in dem Verbund auf Slaves repliziert. Werden Daten im Master verändert, werden diese innerhalb einer Transaktion auch auf den Slaves angepasst. Zu Beachten ist hier das CAP-Theorem, welches besagt, dass maximal zwei der drei Eigenschaften Konsistenz, Verfügbarkeit und Ausfalltoleranz  erreichbar sind. Somit geht Realisierung einer hohen Verfügbarkeit zu Lasten der Konsitent oder der Verfügbarkeit. Das CAP-Theorem meint mit Verfügbarkeit im Sinne akzeptabler Antwortzeiten.

### Räumlich-zeitliche Skalierbarkeit
Die Fähigkeit die Daten strukturiert in einer Tabelle abzulegen bietet auch die Möglichkeit Relationen zu den Daten hinzuzufügen. In der relationalen Datenbank würden somit z.B. der Standort über eine Relation den einzelnen Messwerten bzw. den Geräten hinzugefügt werden. mittels JOIN-Abfragen können diese wieder in eine Struktur gebracht werden. Wie mehrfach bereits angesprochen geht diese strukturierte Speicherung zulasten der Geschwindigkeit.

### Schnell und zuverlässig
Die Geschwindigkeit der Beantwortung von Abfragen hängt bei relationalen DBMS stark von deren Struktur und Komplexität ab. Während bei kleineren Datenbanken noch mit Indizies eine Beschleunigung erreicht werden kann, ist dies bei sehr großen Datenbanken schlecht realisierbar, wenn die Indizies nicht mehr komplett in den Hauptspeicher passen. [Artikel ...]
Ein Master/Slave Verbund kann zwar bei Leseoperationen einen Performancegewinn liefern, hat aber Schwächen bei den Geschwindigkeit von Schreiboperationen, welche bei mehreren tausenden IoT-Geräte zum Flaschenhals werden wird.

### Systemreife
Relationale DBMS sind bereits lange im Einsatz und besitzen somit einen guten Grad an Systemreife. Sie bieten umfangreiche Schutzmechanismen mit Authentifizierung und Authorisierung, teilweise sogar auf Tabellenspalten-Ebene.




## schemafreies Datenbanksystem
### Skalierbarkeit


### Echtzeitverarbeitung


### heterogene Nutzdaten


### Transaktionssicherheit


### Hohe Verfügbarkeit


### Räumlich-zeitliche Skalierbarkeit


### Schnell und zuverlässig


### Systemreife

