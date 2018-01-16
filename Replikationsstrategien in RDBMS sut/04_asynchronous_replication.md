# Asynchrone Replikation

Die asynchrone Replikation ist das einfachste Verfahren und kann meist ohne großen Mehraufwand realisiert werden. Die Daten werden hier in eine primäre Datenbank geschrieben und zu einem späteren Zeitpunkt in das Replikat übertragen. Der Latenzzeitraum kann hierbei klein sein (z.B. fünf Minuten), je nach Bedarf kann aber auch ein großer Zeitraum gewählt werden (z.B. täglich). Bei dem Einsatz eines Replikationsservers ist die Latenz idealerweise der Zeitintervall, die das Datenbanksystem braucht um Transaktionslogs zu schreiben (SAP, 2015).

Asynchrone Replikation kann sowohl uni- als auch bidirektional betrieben werden. Die Probleme der bidirektionalen Replikation und verschiedene Lösungsstrategien werden im Kapitel [Bidirektionale Replikation](06_peer_to_peer.md) aufgegriffen.

**asymmetrisch vs symmetrisch**

**jeweils DBMS nennen**

###Cold Standby

Asynchrone Replikation wird häufig zur Datensicherung (Backup, Replikat 2 in der Abbildung) genutzt. Hierbei kann zu bestimmten Zeitpunkten ein dump (Snapshot) der Datenbank erstellt und in ein Replikat eingespielt werden (Rouse, M. 2015). Diese Strategie nennt sich **snapshot replication** oder **Cold Standby**. Die Übertragung des Snapshots kann z.B. durch FTP oder SSH realisiert werden. Eine Variante dieser Replikationsstrategie ist, dass die Snapshots nur die Änderungen seit dem letzten Update enthalten.

![Asynchrone Replikation](images\Asynchrone Replikation.png)

*Abbildung 1: Asynchrone Replikation Cold Standby*

Wie in Abbildung 1 zu sehen, kann ein Nutzer auf dem Client 1 lesend und schreibend auf die primäre Datenbank zugreifen. Sind in der Masterdatenbank Daten geändert worden, werden diese an die Replikate übertragen, um einen gleichen Datenbestand zu gewährleisten. Falls Nutzer auf ein Replikat zugreifen können (in der Abbildung bei Replikat 1), sollten diese nur lesende Rechte zugewiesen bekommen, da von den Replikaten keine Synchronisation zum Master erfolgt und auch - je nach Latenzzeitraum - nicht die aktuellen Daten gesehen und bearbeitet werden können (Lie, J.S. 2011).

Diese Strategie eignet sich zur Anwendung, wenn nicht an allen Standorten schreibend auf die Datenbank zugegriffen werden muss, oder nur ein Backup erstellt werden soll. Für die initiale Einrichtung eines Replikats bietet sich diese Strategie ebenfalls an (Techopedia, Snapshot).

###Warm Standby

Eine weitere asynchrone Strategie ist der Einsatz eines Replikationsservers. Dieser liest die von der Datenbank erstellten Transaktionslogs aus und gibt die Transaktionen weiter an die Replikate, was zu einer relativ kleinen Latenz bei minimalen Auswirkungen auf die Performanz des primären Systems führt (SAP, 2015). SAP Sybase nennt diese Art der Replikation **Warm Standby**, da es eine Kompromisslösung zwischen **Cold Standby** und **Hot Standby** (siehe [Synchrone Replikation](05_synchronous_replication.md)) darstellt. Da beim Einsatz dieser Stategie die Transaktionen repliziert werden, wird sie auch **transactional replication** genannt. Gegenüber anderen Verfahren gibt es hierbei den Vorteil, dass außer den SQL-Grundbefehlen *update*, *insert*, *delete* und *alter table* auch Transaktionen repliziert werden können, die nicht den Datenbestand, sondern die Datenbanknutzer betreffen (*stored procedures*, SAP, 2015).

![Asynchrone Replikation](images\Asynchrone Replikation.png)

*Abbildung 2: Asynchrone Replikation Warm Standby*

Wie in Abbildung 2 zu sehen ist, kann diese Art der Replikation auch bidirektional erfolgen.

**auf Einsatz eingehen**

### Primary Copy

Bei diesem Verfahren gibt es für jedes Objekt eine Primärkopie. Primärkopien unterschiedlicher Objekte müssen nicht auf demselben Datenbankserver liegen, sie werden dort abgelegt, wo sie am häufigsten bearbeitet und gelesen werden sollen.

![Asynchrone Replikation](images\Asynchrone Replikation.png)

*Abbildung 3: Asynchrone Replikation Primary Copy*

Wie auf der Abbildung zu sehen ist, kann ein Client an einem beliebigen Knoten lesend und schreibend auf die Datenbanken zugreifen. Bearbeitet ein Nutzer einen Datensatz, wird die Bearbeitung am aktuellen Knoten gespeichert. Liegt hier die Primärkopie, wird der Satz geändert und die Änderung an alle weiteren Replikate geendet. Wurde eine Sekundärkopie bearbeitet, wird die Änderung an den Server mit der Primärkopie gesendet, der die Lese- und Schreibsperren verwaltet. Dieser 

**Insgesamt bestehen mehrere Alternativen bei der Realisierung eines Primary-Copy-Protokolls. Ein Ansatz besteht darin, wie beim ROWA-Protokoll Änderungssperren verteilt bei allen kopienhaltenden Rechnern anzufordern. Am Transaktionsende wird jedoch nur die Primärkopie synchron aktualisiert, so daß eine schnellere Bearbeitung von Änderungstransaktionen als mit dem ROWA-Ansatz erreicht wird. Nach Aktualisierung der Primärkopie geht die Schreibsperre dann quasi in den Besitz des Primary-Copy-Rechners über, der diese bis nach der Aktualisierung der Replikate hält. Das verteilte Sperren aller Kopien bringt den Vorteil mit sich, daß Leser wie im ROWA-Verfahren ein beliebiges Replikat referenzieren (sperren) können, nicht also notwendigerweise die Primärkopie. Die verzögerte Aktualisierung der Replikate kann dafür vermehrte Sperrkonflikte verursachen.**

**Die Alternative zu diesem Ansatz besteht darin, Schreibsperren nur noch zentral beim jeweiligen Primary-Copy-Rechner anzufordern, um den Aufwand verteilter Schreibsperren zu umgehen. Für Leser besteht jetzt jedoch das Problem, daß die lokalen Kopien aufgrund der verzögerten Aktualisierung möglicherweise veraltet sind. Für die Behandlung der Lesezugriffe bestehen im wesentlichen drei Alternativen, die sich dadurch unterscheiden, wo die Objektzugriffe erfolgen und wo die Sperren angefordert werden (zentral beim Primary-Copy-Rechner oder verteilt/lokal):**

https://de.wikipedia.org/wiki/Primary_Copy

### Merge Replikation

https://de.wikipedia.org/wiki/Merge-Replikation
