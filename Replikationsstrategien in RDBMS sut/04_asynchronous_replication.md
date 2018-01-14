# Asynchrone Replikation

Die asynchrone Replikation ist das einfachste Verfahren und kann meist ohne großen Mehraufwand realisiert werden. Die Daten werden hier in eine primäre Datenbank geschrieben und zu einem späteren Zeitpunkt in das Replikat übertragen. Der Latenzzeitraum kann hierbei klein sein (z.B. fünf Minuten), je nach Bedarf kann aber auch ein großer Zeitraum gewählt werden (z.B. täglich). Bei dem Einsatz eines Replikationsservers ist die Latenz idealerweise der Zeitintervall, die das Datenbanksystem braucht um Transaktionslogs zu schreiben (SAP, 2015).

###Cold Standby

Asynchrone Replikation wird häufig zur Datensicherung (Backup, Replikat 2 in der Abbildung) genutzt. Hierbei kann zu bestimmten Zeitpunkten ein dump (Snapshot) der Datenbank erstellt und in ein Replikat eingespielt werden (Rouse, M. 2015). Diese Strategie nennt sich **snapshot replication** oder **Cold Standby**. Die Übertragung des Snapshots kann z.B. durch FTP oder SSH realisiert werden. Eine Variante dieser Replikationsstrategie ist, dass die Snapshots nur die Änderungen seit dem letzten Update enthalten.

![Asynchrone Replikation](images\Asynchrone Replikation.png)

*Abbildung 1: Asynchrone Replikation*

Wie in Abbildung 1 zu sehen, kann ein Nutzer auf dem Client 1 lesend und schreibend auf die primäre Datenbank zugreifen. Sind in der Masterdatenbank Daten geändert worden, werden diese an die Replikate übertragen, um einen gleichen Datenbestand zu gewährleisten. Falls Nutzer auf ein Replikat zugreifen können (in der Abbildung bei Replikat 1), sollten diese nur lesende Rechte zugewiesen bekommen, da von den Replikaten keine Synchronisation zum Master erfolgt und auch - je nach Latenzzeitraum - nicht die aktuellen Daten gesehen und bearbeitet werden können (Lie, J.S. 2011).

Dieses Verfahren eignet sich zur Anwendung, wenn nicht an allen Standorten schreibend auf die Datenbank zugegriffen werden muss, oder nur ein Backup erstellt werden soll. Für die initiale Einrichtung eines Replikats bietet sich diese Strategie ebenfalls an (Techopedia, Snapshot).

###Warm Standby

Eine weitere asynchrone Strategie ist der Einsatz eines Replikationsservers. Dieser liest die von der Datenbank erstellten Transaktionslogs aus und gibt die Transaktionen weiter an die Replikate, was zu einer relativ kleinen Latenz bei minimalen Auswirkungen auf die Performanz des primären Systems führt (SAP, 2015). SAP Sybase nennt diese Art der Replikation **Warm Standby**, da es eine Kompromisslösung zwischen **Cold Standby** und **Hot Standby** (siehe [Synchrone Replikation](05_synchronous_replication.md)) darstellt. Da beim Einsatz dieser Stategie die Transaktionen repliziert werden, wird sie auch **transactional replication** genannt. Gegenüber anderen Verfahren gibt es hierbei den Vorteil, dass außer den SQL-Grundbefehlen *update*, *insert*, *delete* und *alter table* auch Transaktionen repliziert werden können, die nicht den Datenbestand, sondern die Datenbanknutzer betreffen (*stored procedures*, SAP, 2015).

### Primary Copy

Bei diesem Verfahren 

https://de.wikipedia.org/wiki/Primary_Copy

### Merge Replikation

https://de.wikipedia.org/wiki/Merge-Replikation
