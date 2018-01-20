# 2 Kategorisierung und Verbreitung

Es existiert inzwischen eine immense Vielzahl an unterschiedlichsten NoSQL Datenbanken. Auf nosql-database.org wird von Prof. Edlich eine zusammenfassende Auflistung geführt; diese enthält bereits mehr als 220 Einträge. Im Rahmen dieser Abhandlung kann aber sinnvollerweise nur eine kleine Auswahl der wichtigsten und am weitesten verbreiteten NoSQL-Vertreter vorgestellt werden. Die spezifische Auswahl wurde aufgrund der obigen Kategorisierung und größten Installationsbasis der jeweiligen Datenbanksysteme (DBS) getroffen.

## 2.1 Kategorisierung

Man unterteilt üblicherweise NoSQL-Datenkbanken in die folgenden vier Hauptkategorien:


- Spaltenorientierte Datenbanken (Wide-Column-Stores)
- Key-Value-Datenbanken (Key-Value-Stores)
- Dokumentendatenbanken (Document Stores)
- Graphdatenbanken (Graph-Stores)

Darüber hinaus existieren noch weitere speziellere Datenbankmodelle wie z.B.  Objektdatenbanken, verteilte ACID-Datenbanken und Multivalue-Datenbanken. Außerdem gibt es Datenbanksysteme, die zwei oder mehrere dieser Datenbankmodelle kombinieren. Diese werden zu den Multi-Model-Datenbanken gezählt wie z.B Amazon DynamoDB, Microsoft Azure Cosmos DB und OrientDB.

## 2.2 CAP-Theorem  

In relationalen Datenbankmanagementsystemen gilt die Konsistenz als oberstes Ziel. Die Abgeschlossenheit (Atomarität) einer Transaktion (entweder ganz oder gar nicht) ist sehr wichtig und gilt als Voraussetzung für die  Verlässlichkeit von Systemen. Man spricht auch von den ACID-Eigenschaften (englisch für Atomicy, Consistency, Isolation und Durability) 

Wie bereits erwähnt “erkaufen” sich NoSQL-Datenbanken ihre Flexibilität durch die Aufgabe oder Aufweichung einiger der strengen ACID-Bedingungen von relationalen Datenbanken. Allerdings fällt hierbei die Gewichtung bezüglich bestimmter Randbedingungen für Unterschiedliche NoSQL-DBMS durchaus unterschiedlich aus.

Das CAP-Theorem nach Brewer (XXX Referenz) hilft bei dem Verständnis der jeweiligen Gewichtungen weiter und trifft die Aussage, dass in einem massiv verteilten Datenhaltungssystem jeweils nur zwei aus den drei Eigenschaften Konsistenz C (Consistent), Verfügbarkeit A (Available) und Ausfalltoleranz P (Partition-Tolerant) garantiert werden können.

Die mehrzahl der NoSQL Datenbaksysteme bewegen sich in den beiden Bereichen CP und AP und bilden Schnittmengen ab, in dem sie die oberste Priorität Ausfalltoleranz entweder mit hoher Konsistenz oder hoher Verfügbarkeit kombinieren. Eine Ausnahme bildet die Graphdatenbank Neo4J, die eher dem CA Segment zugeordnet werden muss.


|    Kombination   |    Datenbanken  |   
|    ---           |    ---          |   
|   Consistent + Available (CA)    |    relationale DBMS, Neo4J ..   |   
|   Consistent + Partition-Tolerant (CP)   |    Redis, Quorum, BigTable, MongoDB, HBase .. |    
|   Available + Partition-Tolerant (AP)   |   DNS, Dynamo, CouchDB, Cassandra, Riak ..  |   
  

![][img-cap]  
Abb. 2.1: Einordnung der Systeme nach CA,CP,AP

## 2.3 Verbreitung

Das DB-Engines.com Ranking ist eine aktuelle Rangliste der Verbreitung einzelner Systeme, die monatlich aktualisiert wird. Die Liste enthält mehr als 300 Einträge und die Wertungen erfolgen nach einer speziellen Berechnungsmethode. Die Parameter werden wie folgt angegeben:

  - Anzahl der Nennungen des Systems auf Websites
  - Allgemeines Interesse an dem System (Suchhäufigkeit in Google Trends)
  - Häufigkeit von technischen Diskussionen über das System
  - Anzahl an Job-Angeboten, die sich auf dieses System beziehen
  - Anzahl an Profilen in professionellen Netzwerken, in denen das System erwähnt wird
  - Relevanz in sozialen Netzwerken

Theoretisch könnte man auch noch weitere Kennzahlen für diese Bewertung mit heranziehen, etwa die Summe der Umsätze aller Unternehmen, die jeweils eines dieser DBMS verwenden. Allerdings sind solche Kennzahlen nicht zwangsläufig öffentlich und oft benutzen Unternehmen auch parallel mehrere Datenbaken für jeweils unterschiedliche Einsatzzwecke und es ist “von außen” nicht unbedingt ersichtlich, wie stark die jeweiligen Datenbanken am Produktivbetrieb beteiligt sind. Dieser Ansatz wäre somit nicht zieführend.

![][img-ranking]  
Abb. 2.2: Ranking der Systeme auf DB-Engines.com

Die Auswal der in den folgenden Kapiteln betrachteten Datenbanksysteme basiert auf den jeweils erfolgreichsten Kandidaten dieser Rangliste in den entsprechenden Kategorien, z.B. wurden für die Kategorie Spaltenorientierte Datenbanken gemäß dem Ranking die Datenbanken Cassandra und HBase ausgewählt:

![][img-rankingwc]  
Abb. 2.3: Ranking der Systeme gefiltert nach Datenmodell, hier Spaltenorientierte Datenbanken

***

Nächstes Kapitel: [3. Spaltenorientierte Datenbanken][kap3]  

[kap3]:             ./3_spaltenorientierte_db.md "Spaltenorientierte Datenbanken"
[img-cap]:          ./img/cap.png "CAP"
[img-ranking]:      ./img/ranking.png "RANKING"
[img-rankingwc]:    ./img/rank_wc.png "RANKING WC"


