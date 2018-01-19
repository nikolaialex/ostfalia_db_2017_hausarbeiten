# Asynchrone Replikation

Bei der **asynchronen Replikation** werden die Daten in eine primäre Datenbank geschrieben und zu einem späteren Zeitpunkt in das Replikat übertragen. Der Latenzzeitraum kann hierbei klein sein (z.B. fünf Minuten), je nach Bedarf kann aber auch ein großer Zeitraum gewählt werden (z.B. täglich). Bei dem Einsatz eines Replikationsservers ist die Latenz idealerweise der Zeitintervall, die das Datenbanksystem braucht um Transaktionslogs zu schreiben (SAP, 2015).

**Asynchrone Replikation** kann sowohl uni- als auch bidirektional betrieben werden. Die Probleme der bidirektionalen Replikation und verschiedene Lösungsstrategien werden im Kapitel [Bidirektionale Replikation](06_peer_to_peer.md) aufgegriffen.

Diese Verfahren können bei geringeren Konsistenzanforderungen eingesetzt werde, da es hier vorkommen kann, dass auf ältere Objektversionen zugegriffen wird. Es kann ebenfalls keine serielle Abarbeitung von Änderungen garantiert werden, weshalb nachträgliche Konflikt-Behebung notwendig sein kann (Rahm, E. 2011).

Es gibt verschiedene Alternativen zum Weiterleiten der Änderungstransaktionen. Die Transaktion kann **so bald wie möglich (ASAP)** oder durch einen Timer verzögert übertragen werden. Bei Einsatz **asymmetrischer** Verfahren kann die Replikation per **Push** oder **Pull** veröffentlicht werden; bei **Push**-Replikation sendet der Master eine Notifikation an die Replikate, während bei der **Pull**-Replikation die Slaves aktiv nach neuen Aktualisierungen fragen.

Man kann ebenfalls zwischen einfachen Replikation, die eine direkte Kopie darstellt, und abgeleiteter Replikation, die die Ergebnisse von SQL-Operationen betrachtet, unterscheiden.

###Cold Standby

**Cold Standby** ist das einfachste Verfahren und kann meist ohne großen Mehraufwand realisiert werden. Es wird häufig zur Datensicherung (Backup, Replikat 1 in der Abbildung) genutzt. Hierbei kann zu bestimmten Zeitpunkten ein dump (Snapshot) der Datenbank erstellt und in ein Replikat eingespielt werden. Diese Strategie nennt sich **Snapshot Replication** oder **Cold Standby**. Die Übertragung des Snapshots kann z.B. durch FTP oder SSH realisiert werden. Eine Variante dieser Replikationsstrategie ist, dass die Snapshots nur die Änderungen seit dem letzten Update enthalten (Rouse, M. 2015).

![Asynchrone Replikation](images\Cold Standby.png)

*Abbildung 2: Cold Standby*

Wie in Abbildung 2 zu sehen, kann ein Nutzer auf dem Client 1 lesend und schreibend auf die primäre Datenbank zugreifen. Sind in der Masterdatenbank Daten geändert worden, werden diese an die Replikate übertragen, um einen gleichen Datenbestand zu gewährleisten. Falls Nutzer auf ein Replikat zugreifen können (in der Abbildung bei Replikat 2), sollten diese nur lesende Rechte zugewiesen bekommen, da von den Replikaten keine Synchronisation zum Master erfolgt und auch - je nach Latenzzeitraum - nicht die aktuellen Daten gesehen und bearbeitet werden können. Es ist somit ein **asymmetrisches** Verfahren.

Diese Strategie eignet sich zur Anwendung, wenn nicht an allen Standorten schreibend auf die Datenbank zugegriffen werden muss, oder nur ein Backup erstellt werden soll. Für die initiale Einrichtung eines Replikats bietet sich diese Strategie ebenfalls an (Techopedia, Snapshot).

###Warm Standby

Eine weitere asynchrone Strategie ist der Einsatz eines Replikationsservers. Dieser liest die von der Datenbank erstellten Transaktionslogs aus und gibt die Transaktionen weiter an die Replikate, was zu einer relativ kleinen Latenz bei minimalen Auswirkungen auf die Performanz des primären Systems führt (SAP, 2015). SAP Sybase nennt diese Art der Replikation **Warm Standby**, da es eine Kompromisslösung zwischen **Cold Standby** und **Hot Standby** ([Synchrone Replikation](05_synchronous_replication.md)) darstellt. Da beim Einsatz dieser Stategie die Transaktionen repliziert werden, wird sie auch **transactional replication** genannt. Gegenüber anderen Verfahren gibt es hierbei den Vorteil, dass außer den SQL-Grundbefehlen *update*, *insert*, *delete* und *alter table* auch Funktionen repliziert werden können (*stored procedures*, SAP, 2015).

![Warm Standby](images\Warm Standby.png)

*Abbildung 3: Warm Standby*

Wie in Abbildung 3 zu sehen ist, kann diese Art der Replikation auch **bidirektional** erfolgen.

Diese Strategie eignet sich für den Einsatz, wenn auf alle Knoten schreibend zugegriffen werden soll und es keine klare Verteilung bei der Häufigkeit der Zugriffe auf ein Replikat gibt. Ausfallsicherheit und Konsistenz sind bei diesem Verfahren sehr hoch.

### Primary Copy

Bei diesem Verfahren gibt es für jedes Objekt eine **Primärkopie**. **Primärkopien **unterschiedlicher Objekte müssen nicht auf demselben Datenbankserver liegen, sie werden dort abgelegt, wo sie am häufigsten bearbeitet und gelesen werden sollen.

![Asynchrone Replikation](images\Primary Copy.png)

*Abbildung 4: Primary Copy*

Wie auf Abbildung 4 zu sehen ist, kann ein Client an einem beliebigen Knoten lesend und schreibend auf die Datenbanken zugreifen. Bearbeitet ein Nutzer einen Datensatz, wird die Bearbeitung am aktuellen Knoten gespeichert. Liegt hier die **Primärkopie**, wird der Satz geändert und die Änderung an alle weiteren Replikate geendet. Wurde eine **Sekundärkopie** bearbeitet, wird die Änderung zunächst an den Server mit der **Primärkopie** gesendet.

Zur Anforderung der Schreibsperren gibt es nun mehrere Möglichkeiten. Der Knoten, der die Änderung fordert kann, ähnlich wie beim **ROWA**-Verfahren, zunächst von allen Replikaten Änderungssperren angefordert werden. Danach werden allerdings nur der **Primärkopien-Server** und der Server, der die Änderung angefordert hat, synchron aktualisiert. Dies führt zu schnelleren Bearbeitungen von Transaktionen gegenüber dem **ROWA**-Verfahren. Der **Primärkopien-Server** hält daraufhin die angeforderten Änderungssperren, bis alle Replikate aktualisiert sind. Der Vorteil dieser Strategie ist, dass ein beliebiges Replikat angesprochen und gesperrt werden kann, nicht nur der **Primärkopien-Server**. Es können dadurch aber vermehrt Sperrkonflikte auftreten, da die Replikate verzögert aktualisiert werden.

Alternativ können die Schreibsperren immer vom **Primärkopien-Server** angefordert werden. Hierbei kann es aber eventuell passieren, dass ein Lesezugriff wegen der Latenz der Aktualisierung auf einen alten Datenstand zugreift.

Für Lesezugriffe können ebenfalls mehrere Strategien angewandt werden. Um beim Lesen immer den aktuellsten Datenbestand zu garantieren, können alle Lesetransaktionen immer die **Primärkopie** referenzieren und sperren. Daduch fällt jedoch der Vorteil der schnellen Verfügbarkeit einer lokalen Kopie weg, da diese nicht mehr zum Lesen genutzt wird. Um dies zu erhalten kann der Lesezugriff auf die lokale Kopie erfolgen, die Sperre aber beim **Primärkopien-Server** angefordert werden. Wird die Lesesperre angefordert, kann auch gleichzeitig die Aktualität der referenzierten Kopie geprüft und eventuell auf Aktualisierungen gewartet oder auf andere Kopien ausgewichen werden.

Stattdessen können Lesezugriffe auch ganz lokal verwaltet werden. Dadurch kann die Aktualität und Konsistenz der Daten allerdings nicht gewährleistet sein, da eventuell Transaktionsergebnisse von anderen Knoten betrachtet werden müssten, die noch nicht synchronisiert sind.

Dieses Verfahren kann nur eingesetzt werden, wenn alle Replikate (auf denen geschrieben werden soll) sich im gleichen Netz mit dem **Primärkopien-Server** befinden, da alle Knoten zum Schreiben Zugriff auf diesen Datenbankserver benötigen. Fällt dieser aus, können weitere Schreibzugriffe nicht abgearbeitet werden. In diesem Fall kann ein neuer Server als **Primärkopien-Server** bestimmt werden, der zunächst alle seine Kopien auf den aktuellen Stand bringen muss, falls es noch nicht durchgeführte Aktualisierungen gibt (Rahm, E. 1994).

### Merge Replikation

Durch Einsatz von **Merge Replikation** werden die Datenbestände der Replikate "gemischt". Es ist ein **symmetrisches** Verfahren, bei dem bei der Synchronisation die vorgefallenen Änderungen von jedem Knoten an die anderen gesendet werden. Dies führt zu einer hohen Konfliktanfälligkeit, da ohne weitere Konfigurationen die Transaktionen parallel ausgeführt werden, aber die Performanz und Verfügbarkeit wird gegenüber **synchroner Replikation** verbessert. Die vorkommenden Konflikte und deren Lösungsansätze werden im folgenden Kapitel behandelt.

Diese Strategie ist besonders für den Einsatz in mobilen Datenbank Applikationen geeignet, bei der es bei der Erstellung neuer und Bearbeitung alter Datensätze geringes Konfliktpotential gibt (Rahm, E. 2011).

[Synchrone Replikation](04_synchronous_replication.md) | [Bidirektionale Replikation]((06_peer_to_peer.md))