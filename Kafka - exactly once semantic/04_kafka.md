# Kafka im Detail

Um Apache Kafka im Detail zu verstehen ist es erforderlich seine Bestandteile zu kennen und einige Konzepte zu verstehen. In Kafka wird eine Dateneinheit als Nachricht (message) bezeichnet. Für Kafka sind Nachrichten einfach nur serialisierte Bytes ohne ein bestimmtes Format. Ein Schema für die Daten kann man mit einem zusätzlichen System wie Apache Avro zur Serialisierung erreichen. Zusätzlich kann eine Nachricht einen Key besitzen. Der Key benötigt ebenfalls kein bestimmtes Format für Kafka. Er liefert mehr Kontrolle darüber wie Nachrichten nach Kafka geschrieben und verwaltet werden. Wird ein Key für Nachrichten verwendet, stellt Kafka sicher, dass Nachrichten mit demselben Key auch in derselben Partition landen. Nachrichten werden der Reihe nach in Kafka wie in eine Art Log geschrieben. Mit Log ist hierbei kein Log einer Anwendung gemeint, wie es von einem Logging-Framework generiert würde, sondern eine Abstraktion eines Speicherformats. Es ist eine Folge von zeitlich geordneten Nachrichten, an die nur weitere Nachrichten an das Ende angehängt werden können. Durch diese Reihenfolge besitzt das Log ein Verständnis von Zeit. Nachrichten am Anfang des Logs sind zeitlich älter als Einträge die neu hinzu kommen. Die Position einer Nachricht in diesem Log kann als Zeitstempel der Nachricht angesehen werden.

Nachrichten werden in sogenannten **Topics** organisiert. Ein Topic ist in etwa wie eine Tabelle in einer Datenbank. Dadurch lassen sich unterschiedliche Arten von Nachrichten in Kafka sauber voneinander trennen und abfragen. Ein Kafka Cluster kann so verschiedene Systeme bedienen ohne die Ereignisse dieser Systeme zu vermischen. Ein Topic könnte so beispielsweise Besuche der Unternehmenswebseite beinhalten, während ein anderes Topic Transaktionen aus dem Webshop des Unternehmens beinhaltet.

Topics werden in **Partitionen** geschrieben. Eine Partition kann man als einzelnes Log im Sinne der oberen Erklärung bezeichnen. Nachrichten werden in der Partition angehängt und sie von der ersten Nachricht an gelesen. Ein Topic verteilt sich normalerweise über mehrere solcher Partitionen. Werden Einträge in ein Topic geschrieben, gibt es keine Garantie, dass die Einträge über die Partitionen hinweg sortiert sind. So kann ein Eintrag in Partition 0 landen, der nächste in Partition 4 und ein weiterer in Partition 2. Über die Partitionen wird auch die Redundanz der Daten sichergestellt und Partitionen können über mehrere Server verteilt werden. Wie zuvor beschrieben, kann die Nutzung eines Keys sicherstellen, dass Kafka Einträge mit demselben Key in die gleiche Partition schreibt. [NarkhedeShapiraPalino2017-07]

Da Kafka stets als Cluster auf einem oder mehreren Servern betrieben wird, besitzt es mehrere Bestandteile, die hier kurz aufgelistet sind:

* Zookeeper für das Management des Clusters
* Broker zur Verwaltung und zum Festhalten der Daten
* Producer für das Eingeben der Daten in Kafka
* Consumer für das Auslesen der Daten aus Kafka

Diese einzelnen Teile von Kafka werden in den folgenden Abschnitten ausführlicher erläutert.

## Zookeeper

Apache Zookeeper ist eine eigene Anwendung zum zentralen Management verteilter Systeme. Zookeeper kümmert sich dabei unter anderem um die Verwaltung von Konfigurationen, Benennung von Knoten im Cluster oder das Management von Gruppen. Was Zookeeper zur Verfügung stellt, wird in den meisten verteilten Systemen benötigt. Es kann sich aber als sehr komplex herausstellen, wenn man eine eigene Implementation der von Zookeeper zur Verfügung gestellten Dienste anstrebt. Durch diese Komplexität kann es dann dazu kommen, dass eine Implementation solcher Dienste zur Verwaltung der verteilten Systeme vernachlässigt wird. Dadurch werden die Systeme brüchig und kleinste Änderungen am Cluster zu einem größeren Problem. Zookeeper stellt als zentraler Service eine Schnittstelle zur Verfügung, um diesen Verwaltungsmehraufwand zu reduzieren und verteilte Systeme wartbarer zu gestalten. Zookeeper wird mit Kafka zusammen ausgeliefert und macht damit das Management der einzelnen Bestandteile von Kafka einfacher. [zookeeper-desc]

## Broker

Ein einzelner Kafka Server wird als Broker bezeichnet. Ein Broker empfängt die Nachrichten der Producer versieht sie mit einem Offset und speichert sie auf der Festplatte. Von einem Broker beziehen die Consumer auch ihre Daten. Das **Offset** ist die Position des Eintrags in der Partition. Ein einzelner Broker im Cluster wird automatisch zusätzlich als Controller bestimmt. Dieser Broker kümmert sich dann um administrative Tätigkeiten und das Management der anderen Broker. Er weist Partitionen zu und kümmert sich um das Monitoring der weiteren Broker. Eine Partition eines Topics gehört stets zu einem Broker, der dann der Leader für diese Partition ist. Weitere Broker, die sich um diese Partition kümmern, sorgen damit für die Replikation dieser Partition. [NarkhedeShapiraPalino2017-07]

## Producer

Ein Producer ist dafür verantwortlich Daten nach Kafka zu übertragen. Eine Anwendung nutzt die für Kafka zur Verfügung stehenden Bibliotheken, um mit der von Kafka zur Verfügung gestellten API einen Producer zu implementieren. Es existieren aber auch eigenständige Producer, wie der Console Producer, den Kafka mitliefert und mit dem sich über die Kommandozeile Nachrichten an einen Kafka-Cluster in ein bestimmtes Topic schicken lassen. [NarkhedeShapiraPalino2017-07]

## Consumer

Die den Producern in Kafka gegenüberliegende Seite sind die Consumer. Ein Consumer liest die Daten aus dem Kafka-Cluster. Auch ein Consumer wird über die von Kafka zur Verfügung gestellten Bibliotheken in der eigenen Anwendung implementiert. So kann beispielsweise ein Consumer in einer Anwendung implementiert werden um Daten aus Kafka in einem Web-Frontend zu visualisieren oder um die Daten in einer wie auch immer gearteten Datenbank zu persistieren. Consumer lesen entweder das komplette Topic von Anfang an ab ihrem Start aus, oder speichern sich das Offset des letzten Eintrags den sie erfolgreich ausgelesen haben, um so im Falle eines Ausfalls ab diesem Offset weiterlesen zu können. [NarkhedeShapiraPalino2017-07]

## Connector

In Kafka existiert zusätzlich zur API für Consumer und Producer auch eine Connector API, die im Prinzip wiederverwendbare Producer und Consumer zur Verfügung stellt. So müssen beispielsweise keine eigenen Consumer oder Producer implementiert werden, die Daten in eine relationale Datenbank wie PostgreSQL schreiben oder auslesen. Connector-Implementationen werden dabei für Producer als "Source" und für Consumer als "Sink" bezeichnet. So existiert beispielsweise ein Connector für die Twitter API, bei der man sich nur um die Konfiguration und nicht die spezifischen Details der API kümmern muss. [kafka-connect-twitter]

## Log Compaction

Da für Kafka Logs nicht unendlich viel Speicher zur Verfügung steht, besitzt Kafka Methoden zur Bereinigung seiner Datenbasis. Dabei werden zwei verschiedene Varianten von Daten unterschieden. Event Daten, die nicht voneinander abhängig sind, wie Seitenaufrufe oder Einträge aus Webserver-Logs und auf der anderen Seite Einträge für Status-Änderungen zu einem spezifischen Schlüssel, wie mehrere Updates eines Werts in einem Schlüssel-Wert Paar. Dazu zählt beispielsweise das Update einer E-Mail-Adresse.

Für Event Daten wird ein Zeit- oder Daten-Fenster erhalten, welches beispielsweise die letzte Stunde oder die letzten 2 Gigabyte vorhält. Ältere Event Daten werden bei solch einer Konfiguration gelöscht. Dieses Verhalten wird über die "retention" gesteuert. An den Brokern gibt es dafür einen Default-Wert, der aber auch an Topics individuell konfiguriert werden kann.

Für die Verringerung der Datenmenge bei Update-Daten wird ein Feature namens "Log Compaction" von Kafka genutzt. Bei Update-Daten ist mitunter nur das aktuellste Update relevant. Bei aktivierter Log Compaction werden ältere Updates für einen Key entfernt. So wird beispielsweise bei mehreren Updates einer E-Mail-Adresse nur das letzte Update beibehalten, da nur der aktuelle Wert für diesen Anwendungsfall von Bedeutung ist. [NarkhedeShapiraPalino2017-07]
