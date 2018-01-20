# Fazit

Die verschiedenen Replikationsverfahren haben Vor- und Nachteile, die man gegeneinander aufwiegen sollte.

| Strategie             | Vorteile                                 | Nachteile                                |
| :-------------------- | :--------------------------------------- | :--------------------------------------- |
| Cold Standby          | Geringer Aufwand<br />Kein Einfluss auf Performanz | hohe Latenzzeiten<br />Keine Kompatibilität mit anderen Datenbanken |
| Warm Standby          | Geringer Einfluss auf Performanz<br />Fehlerresistent<br /> | Latenzzeit hängt von der Netzwerkgeschwindigkeit ab |
| Primary Copy          | Geringer Einfluss auf Performanz<br />Geringe Latenz | Konflikte bei Ausfall des Primärkopien-Servers |
| Merge Replikation     | Gut für mobile Anwendungen               | Hohes Konfliktpotential                  |
| Synchrone Replikation | Keine Latenz<br />Alle Replikate aktuell | Hoher Aufwand<br />Hoher Overhead<br />  |

Wie auch am Beispiel gezeigt, gibt es nicht "das eine" Verfahren, mit dem man am Besten beraten ist, sondern man sollte situationsbedingt analysieren, was man braucht und manchmal eigene geeignete Strategien entwerfen. 

[Fallbeispiel](07_example.md) | [Literaturverzeichnis](09_references.md)