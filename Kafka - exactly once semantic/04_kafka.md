# Kafka im Detail

TODO: @Sebastian Zookeeper, Broker, Producer, Consumer, Offsets, Topics, Partitions
TODO: @Sebastian Quellen einfügen

Eine Dateneinheit wird in Kafka als Nachricht bezeichnet. Für Kafka sind Nachrichten einfach nur Bytes ohne ein bestimmtes Format. Zusätzlich kann eine Nachricht einen Key besitzen. Der Key benötigt ebenfalls kein bestimmtes Format für Kafka. Er liefert mehr Kontrolle darüber wie Nachrichten nach Kafka geschrieben und verwaltet werden. Wird ein Key für Nachrichten verwendet, stellt Kafka sicher, dass Nachrichten mit demselben Key auch in derselben Partition landen.

Nachrichten werden in sogenannten Topics organisiert. Ein Topic ist in etwa wie eine Tabelle in einer Datenbank. Dadurch lassen sich unterschiedliche Arten von Nachrichten in Kafka gut voneinander trennen und abfragen.

Da Kafka stets als Cluster betrieben wird, besitzt es mehrere Bestandteile, die hier kurz aufgelistet sind:

* Zookeeper für das Management des Clusters
* Broker zum Festhalten der Daten
* Producer für das Eingeben der Daten in Kafka
* Consumer für das Auslesen der Daten aus Kafka

Die einzelnen Teile von Kafka werden in den folgenden Abschnitten näher erläutert.

## Zookeeper

Apache Zookeeper ist eine eigene Anwendung zum zentralen Management verteilter Systeme. Zookeeper kümmert sich dabei unter anderem um die Verwaltung von Konfigurationen, Benennung von Knoten im Cluster oder das Management von Gruppen. Was Zookeeper zur Verfügung stellt, wird in den meisten verteilten Systemen benötigt. Es kann sich aber als sehr komplex herausstellen, wenn man eine eigene Implementation der von Zookeeper zur Verfügung gestellten Dienste anstrebt. Durch diese Komplexität kann es dann dazu kommen, dass eine Implementation solcher Dienste zur Verwaltung der verteilten Systeme vernachlässigt wird. Dadurch werden die Systeme brüchig und kleinste Änderungen am Cluster zu einem größeren Problem. Zookeeper stellt als zentraler Service eine Schnittstelle zur Verfügung, um diesen Verwaltungsmehraufwand zu reduzieren und verteilte Systeme wartbarer zu gestalten. Zookeeper wird mit Kafka zusammen ausgeliefert und macht damit das Management der einzelnen Bestandteile von Kafka einfacher.

## Broker

Ein einzelner Kafka Server wird als Broker bezeichnet. Ein Broker empfängt die Nachrichten der Producer versieht sie mit einem Offset und speichert sie auf der Festplatte. Ein Broker liefert außerdem die Nachrichten an die Consumer aus.

TODO: @Sebastian Kapitel zu Brokern mit Topics, Offsets und Partitions

## Producer

Ein Producer ist dafür verantwortlich Daten nach Kafka zu übertragen. Eine Anwendung nutzt die für Kafka zur Verfügung stehenden Bibliotheken, um mit der von Kafka zur Verfügung gestellten API einen Producer zu implementieren. Es existieren aber auch eigenständige Producer, wie der Console Producer, den Kafka mitliefert und mit dem sich über die Kommandozeile Nachrichten an einen Kafka-Cluster in ein bestimmtes Topic schicken lassen.

TODO: @Sebastian Beispiel-Implementation eines Producers

## Consumer

Die den Producern in Kafka gegenüberliegende Seite sind die Consumer. Ein Consumer liest die Daten aus dem Kafka-Cluster. Auch ein Consumer wird über die von Kafka zur Verfügung gestellten Bibliotheken in der eigenen Anwendung implementiert. So kann beispielsweise ein Consumer in einer Anwendung implementiert werden um Daten aus Kafka in einem Web-Frontend zu visualisieren oder um die Daten in einer wie auch immer gearteten Datenbank zu persistieren.

TODO: @Sebastian Beispiel-Implementation eines Consumers

## Connector

In Kafka existiert zusätzlich zur API für Consumer und Producer auch eine Connector API, die im Prinzip wiederverwendbare Producer und Consumer zur Verfügung stellt. So müssen beispielsweise keine eigenen Consumer oder Producer implementiert werden, die Daten in eine relationale Datenbank schreiben oder auslesen. Connector-Implementationen werden dabei für Producer als "Source" und für Consumer als "Sink" bezeichnet.

## Log Compaction

Da für Kafka Logs nicht unendlich viel Speicher zur Verfügung steht, besitzt Kafka Methoden zur Bereinigung seiner Datenbasis. Dabei werden zwei verschiedene Varianten von Daten unterschieden. Event Daten, die nicht voneinander abhängig sind, wie Seitenaufrufe oder Einträge in Anwendungslogs und auf der anderen Seite Einträge für Status-Änderungen zu einem spezifischen Schlüssel, wie mehrere Updates eines Werts in einem Schlüssel-Wert Paar. Dazu zählt beispielsweise das Update einer E-Mail-Adresse.

Für Event Daten wird ein Zeit- oder Daten-Fenster erhalten, welches beispielsweise die letzte Stunde oder die letzten 2 Gigabyte vorhält. Ältere Event Daten werden gelöscht.

Für die Verringerung der Datenmenge bei Update-Daten wird ein Feature namens "Log Compaction" von Kafka genutzt. Bei Update-Daten ist mitunter nur das aktuellste Update relevant. Bei aktivierter Log Compaction werden ältere Updates für einen Key entfernt. So wird beispielsweise bei mehreren Updates einer E-Mail-Adresse nur das letzte Update beibehalten, da nur der aktuelle Wert für diesen Anwendungsfall von Bedeutung ist.
