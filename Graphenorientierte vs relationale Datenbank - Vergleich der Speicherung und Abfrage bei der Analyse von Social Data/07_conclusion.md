Fazit
=====

Das Ergebnis der Analysen zeigt bezüglich der Performanz, dass die gewählten
Abfragen mit dem SQL *Server* in vielen Fällen in weniger Zeit durchgeführt
werden können. Der Grund für die Geschwindigkeit sind allerdings die Abfragen
selbst gewesen. So sind hauptsächliche Abfragen verglichen worden, deren
Erstellung in beiden Datenbanken möglich ist. Gerne wären weitere
graphspezifische Beispiele, wie die Berechnung des *PageRanks*, der
*Betweenness* und *allShortestPath* in die Hausarbeit aufgenommen worden. In
hier nicht weiter beschriebenen Tests liefen diese in der Graphdatenbank sehr
performant ab, die zugehörigen relationale Abfragen zu Erstellen hätte
allerdings den Umfang dieser Hausarbeit weiter erhöht, teilweise sind die
relationalen Entsprechungen der Abfragen nicht möglich. Außerdem sollte
berücksichtigt werden, dass der Autor bereits mehrere Jahre SQL Erfahrungen
besitzt und die Abfragesprache *Cypher* lediglich im Rahmen dieser Arbeit
verwendet wurde.

Im Gegensatz zur Geschwindigkeit ist die Erstellung der Abfragen ­– in dieser
Arbeit in *Cypher* – bei graphbasierten Datenbanken weniger komplex. Die
Abfragen sind durch weniger Wiederholungen und die daraus resultierende
Übersichtlichkeit wartungsfreundlicher und auch sehr gut für Einsteiger
geeignet.

Diese Arbeit ist ein weiteres Beispiel dafür, dass es nicht möglich ist, die
beste Datenbank für alle Anwendungsfälle zu finden. Jeder Datenbanktyp,
*relational* oder *graphbasiert*, aber auch *Dokument* oder *Key-Value* ist für
bestimmte Anwendungsfälle die geeignetste. Diese sollte dafür bei jeder
Anforderungsanalyse ermittelt werden, anstatt sich auf eine Datenbank für alles
zu verlassen.

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