# Synchrone Replikation

1-Kopien-Äquivalenz: 

- vollständige Replikationstransparenz: jeder Zugriff liefert jüngsten transaktionskonsistenten Objektzustand 
- alle Replikate sind wechselseitig konsistent
- Serialisierung von Änderungen
- Konsistenz

Bei Systemen, die aus Gründen der Ausfallsicherheit einen immer synchronen Datenbestand brauchen, kann synchrone Replikation eingesetzt werden. Hierbei müssen alle Komponenten redundant vorhanden sein, was meist hohe Kosten und hohen Aufwand verursacht (SAP, 2015). Synchrone Replikation wird auch **Hot Standby** genannt und ist grundsätzlich ein bidirektionales Verfahren, und braucht daher Konfliktlösungsstrategien.

![Synchrone Replikation](images\Synchrone Replikation.png)

*Abbildung 1: Synchrone Replikation*

###Read-One-Write-All (ROWA)

Wenn ein Nutzer bei Einsatz dieser Strategie einen Datensatz bearbeitet oder einfügt, wird das Commit (**erläutern**) erst durchgeführt, wenn  alle Datenbanken ein Acknowledgement an die Sendende geschickt haben. Die Transaktion wird somit auf allen Datenbankservern gleichzeitig ausgeführt oder gar nicht. Dies sichert einen immer auf allen Systemen synchronen Datenbestand und dadurch kann bei Ausfall eines Knotens ein anderer direkt mit demselben Datenbestand einesetzt werden.

Fällt eines der Systeme aus, kann das **ROWA-Verfahren** nicht weiter funktionieren, da nicht alle Acknowledgements empfangen werden können. Einen Abwandlung, das **ROWAA** (Read-One-Write-All-Available), wird verwendet, um nur bei allen tatsächlich verfügbaren Replikate die Daten zu ändern. Zeitweise ausgefallene Datenbankserver werden mittels protokollierten Änderungen nach einem Neustart wieder synchronisiert. Dieses Verfahren braucht allerdings eine zusätzliche Validierung **weiter ausarbeiten** der Systeme, was einen erhöhten Aufwand verursacht.

Probleme können bei diesem Verfahren auftreten, falls zwischen den Datenbanken zu viel Distanz besteht, da das Acknowledgement nicht schnell genug übertragen werden kann (Rouse,M. 2016). Dieses Verfahren verlangsamt ebenfalls proportional zu der Anzahl der Replikate die Performanz der Schreibfunktion. Es ist also hauptsächlcih für den Einsatz geeignet, wenn es eine geringe Anzahl an Knoten oder Datenänderungen geben soll, und alle verteilten Systeme sich in relativer Nähe zueinander befinden.

Diese Strategie eignet sich zum Einsatz, wenn Ausfallsicherheit und Datenkonsistenz die wichtigsten durch Replikation zu erreichenden Kriterien sind.

**Welche DBMS? "Atomare Transaktionen"**

**Quellen**

**Bearbeitungssperren**

[Einleitung](03_introdction.md)|[Asynchrone Replikation]((05_asynchronous_replication.md))