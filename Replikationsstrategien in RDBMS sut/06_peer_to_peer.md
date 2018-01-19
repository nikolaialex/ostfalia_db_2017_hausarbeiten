# Bidirektionale Replikation

*"Multimaster replication (also called peer-to-peer or n-way replication) enables multiple sites, acting as equal peers, to manage groups of replicated database objects. Each site in a multimaster replication environment is a master site, and each site communicates with the other master sites."* (Oracle, 2007 Seite 1-4)

###Konflikte

Grundsätzlich können alle Replikationsverfahren bidirektional betrieben werden. Im Gegensatz zur unidirektionalen Replikation können hier verschiedene bei zeitgleichen oder zeitnahen Transaktionen Synchronisationskonflikte auftreten, die im Folgenden aufgeführt werden(Oracle, 2007 Seite 1-4).

* **Update Konflikte**:

  Dieser Konflikt tritt auf, wenn zwei Updates von unterschiedlichen Knoten aus dieselbe Zeile einer Datenbank bearbeiten wollen.

* **Eizigartigkeitskonflikte**:

  Wird an zwei Knoten jeweils ein neuer Datensatz in einer Tabelle zerstellt und diesen derselber Primary Key zugeordnet (z.B. bei numerisch aufsteigender Generierung der Primary Keys), führt die Replikation dieser Datensätze zu einem **Einzigartigkeitskonflikt**.

* **Löschkonflikte**:

  **Löschkonflikte** treten auf, wenn ein Datensatz an einem Knoten gelöscht wird und von einem anderen Knoten ebenfalls bearbeitet oder gelöscht werden soll, da der Datensatz für die zweite Transaktion nicht mehr vorhanden ist.

* **Datenkonflikte und Transaktionsreihenfolge**:

  Gibt es mehr als zwei master sites (**Begriff erläutern**), können Konflikte bei der Reihenfolge der Transaktionen auftreten. Wird z.B. an einem Knoten ein Datensatz geändert und diese Änderung an die anderen Knoten repliziert, kann es vorkommen, das an einem der anderen Knoten die Transaktion zunächst blockiert wird. Die auszuführenden Transaktionen werden zu einem späteren Zeitpunkt ausgeführt, hierbei kann es passieren, dass die Transaktionen nicht in derselben Reihenfolge wie auf den anderen Knoten durchgerührt wird und es so zu Konflikten kommt. Es kann hierbei zu **Update Konflikten** und **Löschkonflikten** führen, zusätzlich kann auch ein **Referenzkonflikt** auftreten, falls in einer Transaktion ein Datensatz referenziert wird, der noch nicht vorhanden ist.

Konflikte werden bei den meisten Replikationsverfahren in ein **Error Log** geschrieben und sind daher zurückverfolgbar.

### Konfliktlösungen

Zur Lösung der oben genannten Konflikte gibt es verschiedene Ansätze (Oracle, 2007). 



Timestamp basierte Verfahren:

- „Latest Timestamp“:

  Tritt ein Konflikt zwischen zwei Replikaten bei einem Timestamp
  basierten Replikationsverfahren auf, so wird das Replikat mit dem
  „jüngeren“ Timestamp gewählt. Diese Strategie ist zwar
  Problembehaftet, erfüllt aber trotzdem das Konvergenzkriterium.

- „Earliest Timestamp“:

  Stellt das Gegenteil der „Latest Timestamp“ Methode dar, wählt also
  das „älteste“ Replikat.

- Master/Slave -„Mastercopy wins“:

  Bei Konflikten gewinnt immer das Replikat auf dem Master-Server,
  dies kommt häufig beim „Disconnected Computing“ zum Einsatz

- Site Priority“:

  Bei Prioritätsbasierten Konfliktauflösungsverfahren werden nicht alle

  Replikationsstandardorte als gleichberechtigt erachtet. Dies ist meist

  der Fall, wenn ein Standard den Anspruch auf „präzisereërhebt.

- „Notice User“:

  Wird oft als letzte Möglichkeit angesehen, wenn andere Verfahren

  scheitern wird der Nutzer aufgefordert den entstandenen Konflikt

  manuell aufzulösen

Attributbasierte/Spaltenbasierte Konfliktauflösung

- „Additive and Average“: Bei zwei konkurrierenden Werten wird einfach

  der Durchschnitt der beiden gebildet. Je nach dem, ob man „Additive“
  oder „Average“ verwendet wird dabei auf eine andere Formel
  zurückgegriffen.

- „Minimum and Maximum“: Bei zwei konkurrierenden Werten wird

  einfach das Minimum, bzw. Maximum der beiden gebildet.

- „Priority Group“: Kommt bei konkurrierenden Zugriffen auf eine

  Relation zum Einsatz. Dabei werden die Spalten der Relation mit
  Prioritäten versehen, die Spalte mit der höchsten Priorität bekommt
  als erstes den Zugriff

Referenzkonflikte können durch erneutes Ausführen der fehlerhaften Transaktion, sobald der referenzierte Datensatz vorhanden ist, gelöst werden.

[Voting-Verfahren](https://de.wikipedia.org/w/index.php?title=Voting-Verfahren&action=edit&redlink=1), zum Beispiel [Gewichtetes Votieren](https://de.wikipedia.org/wiki/Gewichtetes_Votieren)


* ​

[Asynchrone Replikation](05_asynchronous_replication.md) | [Fallbeispiel]((07_example.md))

http://www.inf.fu-berlin.de/lehre/SS10/DBS-TA/folien/09-10-TA-Repl-1-1.pdf