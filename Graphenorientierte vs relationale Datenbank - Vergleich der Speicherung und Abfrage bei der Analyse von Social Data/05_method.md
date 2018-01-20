Herangehensweise
================

Für den Vergleich der beiden Datenbanken wird mithilfe des Social Data
Datensatzes Anfragen an beide Systeme gestellt und deren Performance gemessen.
Die Fragestellungen sind durch intensive Tests mit beiden Datenbanken und
einschlägiger Literatur (Bücher über Neo4J & Neo4J Gists[14]) entstanden.

Testumgebung
------------

Beide Datenbanken wurden auf dem gleichen Computer aufgesetzt. Es handelt sich
dabei um einen virtuellen Server, der in einem VMware ESXI Hypervisor
eingerichtet wurde. Die Spezifikationen des virtuellen Servers sind:

* Hardware
    *   Prozessor: virtueller 4-Core Intel® Xeon® CPU E5-2640 \@2,50 GHz
    *   Arbeitsspeicher: 8 GB
    *   Festplatte: 64 GB virtuelle Festplatte (HDD), Thick-Provision Lazy-Zeroed
    *   Grafikkarte ohne 3D Unterstützung mit 8 MB Videoarbeitsspeicher
* Betriebssystem
    *   Microsoft Windows Server 2012 R2 Datacenter (64-bit)
* Software
    *   Microsoft SQL Server 2014 (64-bit)
    *   Neo4J 3.3.1
* Modifizierte Einstellungen in Neo4J Datenbank
    *   dbms.memory.heap.initial_size=2G (statt 512m)
    *   dbms.memory.heap.max_size=4G (statt 1G)

Methode
-------

Es wurden verschiedene Testfälle erarbeitet, die zu vergleichen sind. Für jede
Fragestellungen wurden eine oder mehrere Testabfragen, jeweils für rel.DB und
graph.DB, erstellt. Ausgeführt wurden die Abfragen auf (Teil-)Daten der
Datengrundlage.

In Neo4J wird die zeitl. Länge der Abfrage im Ergebnis ausgegeben, dies ist im
SQL Server Management Studio nicht der Fall. Aus diesem Grund bestand jede
Abfrage aus zwei zusätzlichen currentTimeStamp-Abfragen um die Differenz
zwischen Beginn und Ende jeden Tests zu ermitteln.

Es wären gerne Implementierung von Graphspezifischen Abfragen in SQL erstellt
worden, allerdings hat sich dies als zu aufwendiger Plan herausgestellt. Somit
mussten Algorithmen wie PageRank, Connectness und allShortestPaths anderen, eher
relationalen Abfragen weichen.

Die Werte wurden mithilfe eines Tabellenkalkulationsprogrammes (Microsoft Excel)
gegenüber-gestellt.

Fragestellungen
---------------

Folgende Fragestellungen wurden in Hinblick auf Komplexität der Abfrage und
Dauer der Durchführung in beiden Datenbanken gestellt:

**Wie funktioniert der CSV Import?**

**Wie unterscheiden sich einfache Abfragen?**

**Wie unterscheiden sich kombinierte Abfragen?**

**Wie unterscheiden sich Graph-spezifische Abfragen?**

##### Quellen

\[14]: https://neo4j.com/graphgists/

##### Navigation

| [Deckblatt](00_title.md) |
[Inhalt](00_toc.md) |
[Einleitung](01_introduction.md) |
[Unterschiede der beiden Datenbanktypen](02_db_differences.md) |
[Anwendungsfälle für Graph Datenbanken](03_graphdb_usecases.md) |
[Datengrundlage Social Media](04_data_basis.md) |
[Herangehensweise](05_method.md) |
[Ergebnisse](06_results.md) |
[Fazit](07_conclusion.md) |
[Literaturverzeichnis](08_references.md) |
[Anhang](09_appendix.md) |