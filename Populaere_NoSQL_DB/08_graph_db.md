# 6 Graphdatenbanken

Graphdatenbanken bilden die Relationen zwischen Datensätzen als Graphen ab. Die Datensätze werden Knoten genannt, die Relationen sind Kanten zwischen Knoten. Beide lassen sich mit Eigenschaften, sogenannte Properties versehen und gewichten. Durch spezialisierte Graphalgorithmen lassen sich komplizierte Datenbankabfragen realisieren wie z.B. nächste Nachbarn oder kürzeste Wege finden sowie Regionen starker Vernetzung zu identifizieren.

Die populärsten reinen Graphdatenbanken sind Neo4j, Titan und Giraph. Im Ranking finden sich in dieser Kategorie sonst zahlreiche Mulit-Model DBMS wie Microsoft Azure Cosmos DB, OrientDB, ArangoDB und Virtuoso.


![][img-neo4j]  

## Neo4j

Neo4j ist eine vollumfänglich ACID-transaktionale Graphdatenbank, welche alle Graphdatenstrukturen in einem selbst entwickelten Format speichert. Für die Indexdatenstrukturen kommt dagegen Apache Lucene zum Einsatz. Die Datenbank kann nicht nur als eigenständiger Datenbankserver, sondern auch als eine in eine Java-Anwendung eingebettete Graphdatenbank konfiguriert werden, wodurch eine sehr hohe Verarbeitungsgeschwindigkeit erreicht werden kann. (Edlich 2011)

Neo4j ist nicht nur wegen der hohen Leistungsfähigkeit und der vielen Schnittstellen für die unterschiedlichsten Programmiersprachen, sondern auch aufgrund der vorbildlichen Zusammenarbeit mit ihrer Open Source Community eine der derzeit bekanntesten und beliebtesten Graphdatenbanken aus dem NoSQL-Umfeld.

Ziel von Neo4j ist es, auf Basis des intuitiven Modells „Graph“ Daten leicht verständlich zugänglich zu machen. Dafür hat Neo4j eine eigene Abfragesprache namens Cypher entwickelt. Mittlerweile wird diese auch von anderen Datenbanken unterstützt. (Drost-Fromm 2017)

Effiziente Abfragen ermöglicht ein Query Planner, der ähnlich wie bei relationalen Datenbanken die Anfragen optimiert. Insbesondere JOINs setzt Neo4j sehr effizient um: Statt diese erst zur Query-Zeit zu berechnen, werden Relationen und Beziehungen zwischen Entitäten explizit in Neo4j abgelegt und müssen so nur abgerufen werden.

Neo4J verwendet als Sprachschnittstelle die deklarative Sprache Cypher. Deklarativ heisst in diesem Zusammenhang, dass die Terme ausdrücken, welche Daten gefunden werden sollen aber nicht, wie sie gesucht werden. Cypher basiert auf den Mechanismus des Musterabgleichs. Die Struktur der Ausrücke ist SQL ähnlich, jedoch geschehen die Schemadefinitionen in Cypher implizit. Die wichtigsten Schlüsselwörter sind MATCH, WHERE und RETURN. Die Abfragen ermöglichen das Erstellen, Löschen und Ändern von Knoten, Kanten oder Eigenschaften. Mit openCypher existiert ein Open Source Projekt mit dem Ziel, Graph-Datenbanken weiter zu etablieren.

Neo4j wendet für Replikationen ein Master-Slave-Modell an. Hierbei wird von allen Servern im Cluster ein Write-Master gewählt. Alle anderen Neo4j-Instanzen agieren somit als Read-Slaves. Das bedeutet, dass alle Schreiboperationen von den Read-Slaves synchron auf dem Write-Master abgewickelt werden. Änderungen auf dem Master werden dann asynchron auf die Read-Slaves gespiegelt. Verliert ein Read-Slave den Kontakt mit dem Cluster, kann einfach eine neue Instanz eingekoppelt und auf den aktuellen Stand der Write-Master- Datenbank gebracht werden.

Anwendungen für Graphdatenbanken gibt es in verschiedenen Bereichen, z.B. zur Berechnung von Produktempfehlungen oder in der Logistik.

Im Projekt „Panama Papers“ nutzte das International Consortium of Investigative Journalists (ICIJ) Neo4j zum Speichern der geleakten Dokumente, Metadaten, verknüpften Personen und Unternehmen. 400 Journalisten konnten so auf Graphen-basierte Visualisierungen und Abfragen zurückgreifen, gemeinsam an Dokumenten arbeiten und Muster erkennen.
(Drost-Fromm 2017)



***

Nächstes Kapitel: [7. Sonstige NoSQL Datenbanken][kap7]  

[kap7]:             ./7_sonstige_db.md "Sonstige NoSQL Datenbanken"
[img-neo4j]:      ./img/neo4j.png "NEO"
