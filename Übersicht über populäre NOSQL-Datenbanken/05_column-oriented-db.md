# 3 Spaltenorientierte Datenbanken

## Allgemeines
In der NoSQL-Welt sind verschiedene Datenbankmodelle zu finden. Eines dieser Modelle bilden die sogenannten „Wide-Column Stores. Den Namen verdankt das Modell der Anzahl von bis zu 2 Milliarden Spalten die ein Datensatz aufnehmen kann. Häufig wird dieses Modell im deutschen mit dem Begriff „spaltenorientierte Datenbank“ übersetzt. Diese Übersetzung ist jedoch nur bedingt richtig, da er eigentlich für die relationalen Datenbanksysteme verwendet wird. Daher wird im Laufe dieser Ausarbeitung nur der englische Begriff verwendet.

Das Datenmodell der „Wide Column Stores“ unterscheidet sich stark von den traditionellen, relationalen Datenbanken. Sie sind tabellenähnlich aufgebaut, stellen eine Art zweidimensionale Variante des Key-Value Konzeptes dar und basieren im Grunde auf das BigTable Modell von Google (Chang et. al., 2006). In beiden Modellen existieren die Begriffe Zeile (= Row) und Spalte (= Column). Trotz einer ähnlichen Bedeutung sind sie mit anderen Eigenschaften belegt. Eine Zeile wird mit Hilfe eines Schlüssels identifiziert. Aus diesem Grund ist es möglich, dass mehrere Zeilen eine unterschiedliche Anzahl von Spalten besitzen können. Aufgrund dieser Eigenschaft, eignet sich das Modell besonders für analytische Aufgaben. Die Daten bei diesem Modell werden über Schlüssel gespeichert, die mit Werten verknüpft sind. Eine Spalte bildet dabei die kleine Einheit. Sie besteht immer aus einem Key-Value Paar, sprich einen Namen der mit einem Wert verknüpft ist sowie aus einem Zeitstempel, welcher rein der Versionsverwaltung dient. Man spricht hierbei von einer Spaltenstruktur und ist in Abbildung 1 grafisch dargestellt.

![Spaltenstruktur Allgemein](images/Spaltenstruktur.png)

*Abbildung 1: Spaltenstruktur, eigene Darstellung*

Weisen Spalten eine ähnlichen Wert auf oder soll gemeinsam auf diese zugegriffen werden, werden sie zu sogenannten Spaltenfamilien (= Column Families) zusammengefasst. Diese bilden die Grundeinheit der Zugriffskontrolle und Speicherverwaltung (Chang et. al., 2006).
Es hat sich herausgestellt, dass seltener alle Spalten für eine Zeile benötigt werden, es allerdings Gruppen von Spalten gibt, die häufig zusammenausgelesen werden. Aus diesem Anlass ist es sinnvoll die Daten in Form von Spaltenfamilien zu organisieren. Des Weiteren wird dadurch der Zugriff optimiert. Daten können jedoch erst dann gespeichert werden, wenn entsprechende Spaltenfamilien angelegt wurden. Jede Spalte muss dabei einer Spaltenfamilie zugeordnet sein. Erst im Anschluss ist der Zugriff auf den Spaltenschlüssel möglich.

Datenobjekte werden mit einem Zeilenschlüssel und Objekteigenschaften mit einem Spaltenschlüssel adressiert. In Abbildung 2 ist der Aufbau einer Spaltenfamilie dargestellt.
(1 = Zeilen-Schlüssel, 2= Spalten-Schlüssel, 3 = Spalten-Wert). Spaltenfamilien wiederum können in einem Keyspace zusammengetragen werden (Meier & Kaufmann, 2016).

![Spaltenfamilie](images/Spaltenfamilie.png)

*Abbildung 2: Spaltenfamilie, eigene Darstellung*

Die Verwendung von „Wide Column Stores“ bietet eine Vielzahl von Vorteilen. Sie zeichnen sich durch eine hohe Skalierbarkeit sowie einer hohen Verfügbarkeit durch eine massive Verteilung von Daten. Diese Art von Datenbank wurde für eine Verteilung in großen Clustern konzipiert um sehr große Datenmengen performant verarbeiten zu können. Auch ist ein flexibler Umgang mit unstrukturierten Daten gegeben. Aufgrund eines selektiven Zugriffs auf die nötigen bzw. gewünschten Daten, kommt es zu einer Beschleunigung des Lesezugriffs. Ebenso wirkt es sich positiv auf den Schreibprozess aus (WiWi, 2015). Als Nachteil lässt sich der Schreibprozess über mehrere Spalten nennen. Sollen in einer Studierendendatenbank beispielsweise Daten zu einer Person hinzugefügt werden (Matrikelnummer, Studiengang, Abschlussnote) muss auf mehrere Spalten (= Columns) zugegriffen werden. Dies verlangsamt den Schreibprozess. Auch ist dieses Modell nicht für Graph-Daten geeignet (Baumann, 2015).
Das Wide Column Store Modell entstand aus dem Bedürfnis heraus ein flexibles System mit hoher Perfomenz und Verfügbarkeit beim Umgang mit Daten im Petabyte-Bereich (= 1024 Terabyte) haben zu wollen, die auf tausende Cluster-Knoten verstreut sind. Verwendung findet dieses Datenmodell in den unterschiedlichsten Bereichen. Aufgrund der Beschaffenheit für analytische Aufgaben wird es unter anderen im Data-Mining Bereich genutzt. Basierend auf die Eigenschaft große Mengen an Daten bewältigen zu können, in Bezug auf die Schreib- und Leselast, eignet sich das Modell für den Einsatz bei Web 2.0 Seiten. Wichtige Vertreter sind Youtube, Google, Facebook und Twitter (DB_2014), (Lakshman & Malik, 2010).

Anwendung findet dieses Datenbankmodell auch bei Facebook. Dort ermöglicht es das Durchsuchen der Inbox. Auch ist es dem Modell aufgrund der Struktur möglich, Daten in Abhängig von der Zeit (Zeitstempel) zu erfassen. Weitere Anwendungsgebiete umfassen die Bereitstellung von Kartenmaterial von Google Earth, sowie Teile der Websuche (Chang et. al., 2006).

Auf der Website von DB-Engines (DB, 2018) ist eine Rangliste in Bezug auf die Popularität von Datenbankmanagementsystemen der letzten drei Monate zu finden. Im Bereich der Wide Column Store handelt es sich bei Apache Cassandra, Apache HBase und Microsoft Azure Cosmos DB um die populärsten Datenbanken, wie es in Abbildung 3 zu sehen ist.

![Raking](images/Raking.png)

*Abbildung 3: Ranking Wide Column Stores [DB_2]*
