# Einleitung

Datenbanken stehen heutzutage im Kern vieler Anwengungen und Systeme. Da diese Datenbanken möglichst immer und von vielen Standorten aus erreichbar sein sollen, bietet es sich an, die Daten an den verschiedenen Standorten zu speichern. Dies nennt man ein **verteiltes Dantenbanksystem**, wobei die verschiedenen Standorte als **Knoten** bezeichnet werden.

Die Methode, **verteilte Datenbanksysteme** zu erstellen und aufrecht zu erhalten nennt sicht **Replikation**. Daten können hiermit an mehreren Knoten gespeichert werden und bleiben auch nach Ausfall einer der Datenbankservern erreichbar. Die Geschwindigkeit der Lesezugriffe kann durch Replikation ebenfalls verbessert werden, da auf eine lokale Kopie zugegriffen wird anstatt auf ein entferntes System. Auch die Überlastung einzelner Datenbankserver kann durch verbesserte Lastbalancierung beim Einsatz von Replikation vermieden werden (Rahm, E. 1994). Durch Einsatz von Replikation kann aber "ein nicht unerheblicher Mehraufwand beim Design, der Implementierung und der Wartung des Datenbanksystems" (Rahm, E. 2011) entstehen.

Replikation kann grundsätzlich in relationalen und auch nicht relationalen Datenbankmanagementsystemen (**DBMS**) eingesetzt werden. Im Rahmen dieser Arbeit werden verschiedene Replikationsstrategien in relationalen Datenbanken (**RDBMS**) vorgestellt.

Es gibt verschiedene Verfahren und Ansätze, wie Replikation betrieben werden kann.

Zunächst wird zwischen **vollständiger** und **Teilreplikation** unterschieden. Bei **vollständiger Replikation** sind alle Daten an allen Knotenpunkten vorhanden (voll-redundant), während bei **Teilreplikation** nur ein Teil der Datenmenge repliziert wird. Diese beiden Arten der Replikation können bei allen aufgeführten Replikationsstrategien angewandt werden, daher werden sie nicht weiter besprochen.

Weiterhin kann Replikation nach bestimmten Zeitschemen kategorisiert werden. Bei der **synchronen Replikation** werden Änderungstransaktionen in allen Knoten gleichzeitig durchgeführt, bei Verwendunger **asynchroner Replikation** wird die Änderung erst gespeichert und später an den anderen Knoten durchgeführt  (Rahm, E. 2011).

Schließlich gibt es noch die Unterscheidung zwischen **uni-** und **bidirektionaler Replikation**. **Unidirektionale Replikation** ist nach dem **Master/Slave-Prinzip** aufgebaut, besitzt also einen **Primärknoten**, an dem Daten geändert werden können (auch: **asymmetrische Replikation**). Beliebig viele Knoten können lokale Kopien der Daten vorhalten (**Replikate**), die nicht geändert werden dürfen. Wird **bidirektionale Replikation** eingesetzt, können die Daten an allen Knoten geändert werden (auch: **symmetrische Replikation**). Die Synchronisierung kann mittels verschiedener Verfahren erfolgen, die in den folgenden Kapiteln erläutert werden. Hierbei können verschiedene Probleme bei den Datenbeständen auftreten, auf die im Kapitel [Bidirektionale Replikation](06_peer_to_peer.md) weiter eingegangen wird.



[Inhaltsverzeichnis](02_toc.md) | [Synchrone Replikation]((04_synchronous_replication.md))