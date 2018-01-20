Anwendungsfälle für Graph Datenbanken
=====================================

Der naheliegendste Anwendungsfall ist die Verwendung von Graph Datenbanken in
Applikationen mit sozialen Komponenten. Zudem gibt es eine Reihe weiterer
Anwendungen für die Graph Datenbanken verwendet werden können. In seinem
Vortrag [10] beschreibt Neo4J Mitarbeiter Ryan Boyd die Anwendungsfälle seiner
Kunden:

* Soziale Anwendungen
* Empfehlungen
* Suche und Auffinden
* Netzwerkanalyse in Datenzentren
* Master Data Management
* Identitätsverwaltung
* Geolocation

Die Herausforderungen dieser Anwendungen für relationale Datenbanken und die
Empfehlung der Verwendung einer Graph Datenbank lassen sich nach van Bruggen in
drei Kategorien teilen.

Komplexe Abfragen
-----------------

Komplexe Abfragen treten auf, wenn Informationen aus vielen Quellen (in rel.
Datenbanken Tabellen) miteinander durch *JOINS* oder kartesisches Produkt
verknüpft werden müssen um zum erwünschten Ergebnis zu gelangen. In Graph
Datenbanken werden keine Join-Operationen verwendet, jeder Knoten wird durch
eine Beziehung zum nächsten Knoten ohne Index (siehe Kapitel: Unterschiede der
beiden Datenbanktypen \> Speicherung) gespeichert. Diese Beziehung kann somit
als die konkret gespeicherte Abbildung eines *JOINS* zwischen diesen beiden
Knoten verstanden werden.

In Graph Datenbanken können diese komplexen Abfragen durch eine Art
Musterabgleich (engl. Pattern Matching) gestellt werden. Es entsteht eine hohe
Performanz, da die Abfragen nicht von der Größe des Datensatzes sind sondern von
der Größe der Ergebnismenge abhängen. [11]

Abfragen auf Live Daten
-----------------------

In relationalen Datenbanken werden diese Art von Abfragen vorberechnet und
vorformatiert. Liveabfragen laufen somit auf Duplikaten und
Ent-Normalisierungen. In Graph Datenbanken können diese Redundanzen entfernt
werden und in Echtzeit Abfragen auf dem Originaldatensatz gemacht werden.[12]

Pfadabfragen
------------

In Pfadabfragen wird ebenfalls die Verbindung zwischen den Knoten analysiert.
Dies geschieht um beispielsweise den kürzesten Weg zwischen diesen zu finden.
Dies ist in relationalen Datenbanken nur möglich wenn die Struktur zwischen
jeglichen Knoten in die Abfrage mitgegeben wird. In Graph Datenbanken ist dies
nicht nötig, hier wird lediglich der Start- und der Endknoten definiert. Die
Berechnung, wie der Pfad aussieht, wird von der Datenbank durchgeführt. [13]

Anwendungsfälle anderer Datenbanken
-----------------------------------

Es gibt allerdings auch Anwendungsfälle, die weniger gut in Graph Datenbanken
ausgedrückt werden können. Abfragen auf große Datensätze, die wenige Joins
benötigen oder viele Berechnungen (wie Count, Sum, Avg) durchführen haben in
Graph Datenbanken eine schlechtere Performanz als beispielsweise relationale
Datenbanken. Abfragen die nicht nach lokalen Informationen in einem Graph
suchen, sondern den Graph selbst untersuchen sind ebenfalls nicht gut für die
Verwendung von Graph Datenbanken geeignet. Hierfür gibt es separate,
spezialisierte Werkzeuge. Eine weitere Anwendung, die durch Key-Value Stores
oder Document Stores effizienter bearbeitet werden kann, sind einfache Abfragen,
die mit Lese- oder Schreibanfragen Listensammlungen erstellen.


##### Quellen

\[10]: Boyd (09-2015, ab 2:30 min)

\[11]: van Bruggen (2014, S. 37f.)

\[12]: van Bruggen (2014, S. 39)

\[13]: van Bruggen (2014, S. 39)

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