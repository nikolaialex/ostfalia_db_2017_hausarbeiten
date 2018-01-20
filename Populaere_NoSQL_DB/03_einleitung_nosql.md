# 1 Einleitung 



![][img-nosql]  

## 1.1 NoSQL


Als *NoSQL* Datenbanken werden jene Datenbanksysteme bezeichnet, die deutliche Unterschiede zum klassischen relationalen Datenmodell aufweisen. Der Terminus NoSQL stand zu unterschiedlichen Zeiten für unterschiedliche Begrifflichkeiten und wird heutzutage typischerweise als „Not only SQL“ verstanden, was inhaltlich übersetzt „Nicht nur SQL“ oder „Mehr als nur SQL“ bedeutet. 

NoSQL ging 2009 aus dem Titel des Events „NoSQL Meetup“ hervor, welches sich mit alternativen verteilten Datenspeichersystemen beschäftigte.

Eine mögliche Definition ist nach (Edlich 2011) so formuliert, dass einige – nicht aber zwangsläufig alle - der nachfolgenden Kriterien erfüllt sein müssen um von einer NoSQL-Datenbank sprechen zu können:

- Die Datenbank implementiert ein nicht-relationales Datenmodell
- Sie verfügt über verteilte Speicherung und horizontale Skalierbarkeit
- Sie ist quelloffen
- Sie besitzt ein flexibles oder kein Datenbankschema
- Sie unterstützt Datenreplikation
- Sie besitzt eine einfache API und
- Flexibilität bezüglich der Einhaltung ACID Eigenschaften


Durch die in den letzten Jahrzehnten stark gestiegenen Datenmengen gibt es einen immer größer werdenden Bedarf nach Speicherung, Verarbeitung und Analyse dieser Datenmengen (Big Data). Die Anforderungen an Datenbanken gehen deshalb immer mehr in Richtung der Verwaltung großer Datenmengen gepaart mit einer redundanten und verteilten Speicherung dieser Daten unter Verwendung von leicht skalierbarer Standardhardware. 

![][img-logos]  
Abb. 1.1: Logos einiger populärer NoSQL-Datenbanken

Inzwischen gibt es eine große Zahl existierender NoSQL-Systeme, die individuell jeweils auf unterschiedliche Anwendungsszenarien hin optimiert sind. Nicht selten finden sich im produktiven Einsatz deswegen mehrere Datenbanksysteme im Zusammenspiel. Die Verfügbarkeit der Daten spielt dabei eine zentrale Rolle. Für das Testen der Ausfalltoleranz wurden verschiedene Konzepte ersonnen, beispielsweise kann der von Netflix entwickelte Chaos Monkey das abrupte Ausschalten ganzer Cloud Instanzen simulieren, um die Reaktionen der Lastverteilung nach einem Ausfall zu untersuchen.

Die vorgestellten Datenbanksysteme verzichten auf eine Mehrzahl von Features der relationalen Datenbanksysteme (RDBMS) mit dem primären Ziel, optimal auf preisgünstiger Standardhardware zu skalieren. (Edlich 2011)

Das folgende Kapitel gibt Auskunft über die Kategorisierung und verschiedene Auswahlkriterien der betrachteten NoSQL-Systeme, basierend auf der Ranking-Liste DB-Engines.com. 

Primär ist die Struktur der Daten entscheidend für den schemaflexiblen Ansatz und die Wahl des jeweiligen Datenverwaltungsmodells. Üblicherweise werden vier Hauptkatergorien von NoSQL-Datenbanken unterschieden:


- Spaltenorientierte Datenbanken 
- Key-Value-Datenbanken
- Dokumentendatenbanken 
- Graphdatenbanken

Eine einführende Beschreibung der jeweiligen Datenbankmodelle sowie typische Vertreter dieser vier Kategorien werden in den Kapiteln 3 bis 6 gegeben. Darüber hinaus zählen zu den NoSQL Datenbanken auch einige Systeme, die nicht in diese vier Hauptkategorien passen, oder die mehrere dieser Kategorien kombinieren. Kapitel 7 liefert einen abschließenden Ausblick auf diese speziellen Varianten.


***

Nächstes Kapitel: [2. Kategorisierung und Popularität][kap2]  

[kap2]:             ./2_kategorisierung_und_popularitaet.md "Kategorisierung und Popularität"
[img-logos]:      ./img/NoSQLLogos300.jpg "Logos"
[img-nosql]:      ./img/nosql.png "NoSQL"

