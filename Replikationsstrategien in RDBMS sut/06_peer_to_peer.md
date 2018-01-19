# Bidirektionale Replikation

*"Multimaster replication (also called peer-to-peer or n-way replication) enables multiple sites, acting as equal peers, to manage groups of replicated database objects. Each site in a multimaster replication environment is a master site, and each site communicates with the other master sites."* (Oracle, 2007 Seite 1-4)

### Konflikte

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

Zunächst können die Replikate mit **Timestamps** versehen werden, durch den die jeweils aktuellste Kopie erkannt werden kann. Mit der Strategie **Latest Timestamp** gewinnt bei Konflikten immer der jüngste Timestamp, während bei **Earliest Timestamp** immer das älteste Replikat gewählt wird.

Wird ein **asymmetrisches Verfahren** eingesetzt, kann festgelegt werden, dass immer das Replikat gewinnt, das sich auf dem Master-Server befindet. Ebenso kann den Knoten eine Priorität zugeordnet werden, die bei der Strategie der **Site Priority** ausgewertet wird und bei der das am Höchsten priorisierte Replikat gewinnt.

Scheitern alle genannten Verfahren, gibt es die Möglichkeit den Nutzer über den Konflikt zu informieren, damit er manuell gelöst werden kann (**Notice User**).

Weiterhin gibt es Attribut- und Spaltenbasierte Konfliktlösungen. Bei den Strategien **Additive** und **Average** wird beispielsweise der Durchschnitt zweier konkurrierender Werte gebildet - die angewandte Formel richtet sich nach der gewählten Stragie. Ähnlich kann der **Maximum** oder **Minimum** Wert gebildet werden.

Treten Konflikte bei Zugriffen auf Relationen auf, kann der Ansatz der **Priority Group** genutzt werden, bei dem die Spalten der Relation mit Prioritäten versehen werden und anschließend die Spalte mit der höchsten Priorität Zugriff erhält.

Referenzkonflikte können durch erneutes Ausführen der fehlerhaften Transaktion, sobald der referenzierte Datensatz vorhanden ist, gelöst werden (Adler, Y. 2016).

[Asynchrone Replikation](05_asynchronous_replication.md) | [Fallbeispiel](07_example.md)
