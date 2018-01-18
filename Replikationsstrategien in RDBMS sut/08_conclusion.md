# Fazit

Die verschiedenen Replikationsverfahren haben Vor- und Nachteile, die man gegeneinander aufwiegen sollte.

| Strategie                | Vorteile                                 | Nachteile                                |
| :----------------------- | :--------------------------------------- | :--------------------------------------- |
| Cold Standby             | Geringer Aufwand<br />Kein Einfluss auf Performanz | hohe Latenzzeiten<br />Keine Kompatibilität mit anderen Datenbanken |
| Warm Standby             | Geringer Einfluss auf Performanz<br />Fehlerresistent<br />Kompatibilität mit verschiedenen DBMS | Latenzzeit hängt von der Netzwerkgeschwindigkeit ab |
| Primary Copy             |                                          |                                          |
| Merge Replikation        |                                          |                                          |
| Hot Standby              | Keine Latenz                             | Hoher Aufwand<br />Hoher Overhead<br />  |
| Multi-Master Replikation |                                          |                                          |



Es gibt nicht "das Eine" Verfahren, mit dem man am Besten beraten ist, sondern man sollte situationsbedingt analysieren, was man braucht.

[Fallbeispiel](07_example.md) | [Literaturverzeichnis](09_references.md)