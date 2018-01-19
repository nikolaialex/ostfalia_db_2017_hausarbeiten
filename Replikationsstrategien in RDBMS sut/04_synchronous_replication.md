# Synchrone Replikation

Bei Systemen, die aus Gründen der Ausfallsicherheit einen immer synchronen Datenbestand brauchen, kann synchrone Replikation eingesetzt werden. Hierbei müssen alle Komponenten redundant vorhanden sein, was meist hohe Kosten und hohen Aufwand verursacht (SAP, 2015).

Durch **synchrone Replikation** kann eine **1-Kopien-Äquivalenz** erreicht werden, was bedeutet, dass jeder Zugriff (von jedem Knoten) auf die Datenbank den aktuellen Objektzustand liefert. Alle Replikate sind konsistent. Änderungen müssen bei dieser Art der Replikation serialisiert werden, da durch parallele Zugriffe diese Kriterien nicht mehr erfüllt werden können (Rahm, E. 1994).

Synchrone Replikation wird auch **Hot Standby** genannt und ist grundsätzlich ein **bidirektionales** (**symmetrisches**) Verfahren, und braucht daher Konfliktlösungsstrategien.

![Synchrone Replikation](images/Synchrone Replikation.png)

*Abbildung 1: Synchrone Replikation*

###Read-One-Write-All (ROWA)

Wenn ein Nutzer bei Einsatz dieser Strategie einen Datensatz bearbeitet oder einfügt, wird diese Änderung erst durchgeführt, wenn  alle Datenbanken ein Acknowledgement an die Sendende geschickt haben. Die Transaktion wird somit auf allen Datenbankservern gleichzeitig ausgeführt oder gar nicht. Dies sichert einen immer auf allen Systemen synchronen Datenbestand und dadurch kann bei Ausfall eines Knotens ein anderer direkt mit demselben Datenbestand eingesetzt werden.

Die Performanz hinsichtlich Änderungstransaktionen wird jedoch deutlich von diesem Verfahren beeinflusst. Vor jeder Änderung ist eine Schreibsperre auf allen Kopien gefordert, was zusätzlichen Kommunikationsaufwand bedeuten kann. Alle Knoten sind am Commit-Protokoll beteiligt; wird ein Zwei-Phasen-Commit-Protokoll eingesetzt, werden in Phase 1 die Änderungen an alle Knoten übertragen. Die Änderung wird protokolliert und in der zweiten Phase werden die Replikate aktualisiert und die Sperren feigegeben (Rahm, E. 1994).

Fällt eines der Systeme aus, kann das **ROWA-Verfahren** nicht weiter funktionieren, da nicht alle Acknowledgements empfangen werden können. Einen Abwandlung, das **ROWAA** (Read-One-Write-All-Available), wird verwendet, um nur bei allen tatsächlich verfügbaren Replikate die Daten zu ändern. Zeitweise ausgefallene Datenbankserver werden mittels protokollierten Änderungen nach einem Neustart wieder synchronisiert. Dieses Verfahren braucht allerdings eine zusätzliche Validierung der Systeme, was einen erhöhten Aufwand verursacht (Rahm, E. 1994).

Probleme können bei diesem Verfahren auch auftreten, falls zwischen den Datenbanken eine zu große Distanz besteht, da das Acknowledgement nicht schnell genug übertragen werden kann (Rouse,M. 2016).

Dieses Verfahren verlangsamt ebenfalls proportional zu der Anzahl der Replikate die Performanz der Schreibfunktion. Es ist also hauptsächlich für den Einsatz geeignet, wenn es eine geringe Anzahl an Knoten oder Datenänderungen geben soll, und alle verteilten Systeme sich in relativer Nähe zueinander befinden (Wikipedia, 2016). Ausfallsicherheit und Datenkonsistenz sollten die wichtigsten durch Replikation zu erreichenden Kriterien sein und der hohe Mehraufwand muss in Kauf genommen werden können.

### Abstimmungsverfahren

Bei diesem Verfahren wird per Abstimmung bestimmt, ob ein Replikat geändert wird. Es kann auch auf Lesezugriffe angewendet werden, erhöht dann aber die Kommunikationskosten erheblich.

Werden ungewichtete Stimmen gewertet, ist jeder Knoten gleichberechtigt - kann er gesperrt werden, zählt es als stimme. Wird eine bestimmte Anzahl an Stimmen erreicht, wird die Änderungstransaktion durchgeführt. Die zu erreichende Anzahl an Stimmen kann vorher festgelegt werden (**statisches Quorum**), z.B. muss beim **Majory Voting** die Mehrheit der Replikate sperrbar sein, oder sie kann zur Laufzeit bestimmt werden (**dynamisches Quorum**).

Bei Einsatz von gewichteten Stimmen erhält jeder Knoten eine bestimmte Anzahl an Stimmen.

Das statische Quorum ist hier fehleranfälliger, da es vorkommen kann, dass so viele Knoten ausfallen, dass die bestimmte Anzahl an Stimmen nicht mehr erreicht werden kann. Bei gewichteten Stimmen kann dies schnell der Fall sein, wenn ein System ausfällt, das eine hohe Anzahl an Stimmen hat (Rahm, E. 2011 und Rahm, E. 1994).

[Einleitung](03_introduction.md)|[Asynchrone Replikation]((05_asynchronous_replication.md))
