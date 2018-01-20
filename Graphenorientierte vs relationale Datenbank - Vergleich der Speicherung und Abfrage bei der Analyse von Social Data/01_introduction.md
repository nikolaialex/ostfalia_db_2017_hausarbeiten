Einleitung
==========

Die Bedeutung einer Relation erklärt der Duden \[1] folgendermaßen:

1.  *(Fachsprache) Beziehung, in der sich [zwei] Dinge, Gegebenheiten, Begriffe
    vergleichen lassen oder [wechselseitig] bedingen; Verhältnis*
2.  *(Mathematik) Beziehung zwischen den Elementen einer Menge*

Eine *relationale Datenbank* ist – verwendet man die mathematische Bedeutung –
eine Datenbank in der Beziehungen zwischen Elementen vorkommen und, da dies das
einzige beschreibende Attribut dieser Art Datenbank ist, einen wichtigen Teil
ausmachen. Die Speicherung von Informationen geschieht allerdings hauptsächlich
in Tabellenform und Beziehungen werden durch Fremdschlüssel abgebildet. Ein
Fremdschlüssel ist ein Teil des Schemas von Tabellen und verbindet eine Spalte
einer Tabelle mit einer Spalte einer anderen Tabelle. Eine konkrete Relation von
zwei Objekten wird durch das Zusammenführen (*JOIN*) dieser beiden Tabellen
realisiert, also indirekt. Der Begriff *relationalen Datenbank* lässt erwarten,
dass die enthaltenen Relationen direkt und unmittelbar zur Verfügung stehen.
Durch die nötige Verwendung eines Hilfsmittel, um eine Beziehung darzustellen,
ist dies jedoch nicht gegeben.

In dieser Hausarbeit wird daher ein Vergleich zu einem anderen Datenbanktyp, der
Beziehungen zwischen Einzelobjekten konkret abspeichert, gezogen. Hier werden
Objekte und Relationen als eigenständige Schemabestandteile aufgefasst und
verwendet. Objekte werden dabei durch konkrete Relationen verbunden. Die Rede
ist von *graphbasierten Datenbanken*. Mithilfe eines Datensatzes mit Beiträgen
aus den Sozialen Medien wird analysiert inwieweit sich beide Datenbankarten in
ihrer Speicherung und Abfrageverwendung unterscheiden. Da mit Cypher eine neue
Sprache erlernt wurde, wurde bei der Erstellung der SQL Abfragen darauf
geachtet, keine Funktionen oder sonstige programmatischen Lösungen anzuwenden.
Dadurch soll die Vergleichbarkeit der nativen Abfragesprachen erhöht werden.

##### Quellen

\[1]: https://www.duden.de/rechtschreibung/Relation

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