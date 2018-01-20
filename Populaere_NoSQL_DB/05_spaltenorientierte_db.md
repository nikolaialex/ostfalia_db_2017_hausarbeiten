# 3. Spaltenorientierte Datenbanken

Der wesentliche Unterschied zu RDBMS besteht darin, dass spaltenorientierte Datenbanken (Wide Column Stores) ihre Inhalte spaltenweise auf dem Datenträger abspeichern und nicht zeilenweise wie das typsicherweise bei SQL-basierten Datenbanken der Fall ist.

Dadurch müssen für eine Abfrage weniger Blöcke des unterliegenden Datenträgers gelesen werden und somit reduziert sich die Anzahl der Datenträger-Zugriffe und es ergibt sich eine deutlich höhere Lese- und Schreibgeschwindigkeit. Dieses Zugriffsschema wurde speziell für eine zeitoptimierte Auswertung sehr großer Datenmengen entwickelt.

Ein Beispiel soll die spaltenorientierte Zugriffslogik verdeutlichen. Gegeben sei eine Tabelle:


|     |      |           |             |         |
|   ---   |    ---   |  ---   |  ---   |  ---   |
|   1    |   Doe  |  John   |   WRE   |  202   |
|   2    |    Miller    |    Anne       |      NLV       |   135      |
|   3    |    Smith    |     Peter      |       RQN      |    327     |


Anstatt der in relationalen Datenbanken üblichen zeilenorientierten Speicherung

```
1,Doe,John,WRE,202;2,Miller,Anne,NLV,135;3,Smith,Peter,RQN,327
```

werden die Spalten spaltenweise gespeichert, es werden dabei alle Werte einer Spalte direkt hintereinander abgelegt:

```
1,2,3;Doe,Miller,Smith;John,Anne,Peter;WRE,NLV,RQN;202,135,327  
```

Die Vorteile sind leicht erkennbar: Durch die nahe zusammenliegende physische Speicherung der Daten müssen weniger separate Blöcke gelesen und in den Hauptspeicher geladen werden. So können große Datenmengen schnell für Analysen und Berechnungen bereitgestellt werden.

Ein weiteres wesentliches Charakteristikum von spaltenorientierten Datenbanken ist, dass diese die spaltenweise Speicherung dafür ausnutzen können, die Daten vor dem Speichern zu komprimieren und so ohne größere Latenzzeiten eine wesentliche Reduzierung der Datenbankgröße erreichen können.


![][img-cass]  

## Cassandra

Cassandra ist eine spaltenorientierte NoSQL-Datenbank, die von Facebook entwickelt und 2008 als Open Source freigegeben wurde. Sie benötigt kein Hadoop-Cluster[<sup>1][foot33]. Die Architektur der Datenbank kombiniert den Ansatz von Amazons verteilter Key-Value-Datenbank Dynamo zur Speicherung mit dem Datenmodell von Googles Bigtable[<sup>2][foot33].

Cassandra sichert hohe Verfügbarkeit und Ausfalltoleranz zu, macht aber Kompromisse bei der Konsistenz. Ein sogenanntes Konsistenzlevel lässt sich bei Cassandra einstellen. Will man sicher sein, dass ein Lesevorgang stets die aktuellste Version der Daten liefert, muss man das Konsistenzlevel „Quorum“ setzen. Soll ein Schreibvorgang zuverlässig erfolgen, auch wenn nicht alle Knoten online sind, muss man den Konsistenzlevel „ANY“ einstellen. (Drost-Fromm 2017)

Cassandra ermöglicht Replikation über geographisch weite Distanzen. Dadurch wird bei einem Systemausfall die Wiederherstellung von Daten zuverlässig gewährleistet.

Die hervorhebende Eigenschaft von Cassandra ist die Schreibgeschwindigkeit. hierfür werden einige Technologien wie In-Memory-Speicher, AppendOnly Log und  Zwischenspeicherung auf Festplatte kombiniert. Weil die Daten parallel auf mehrere Rechner verteilt werden können, sind hohe Datenraten erreichbar (horizontale Skalierung).

Bei der Abfrage gespeicherter Daten besteht eine architektonische Hürde: mit Cassandra können Daten nur dann schnell gelesen werden, wenn bereits beim anlegen und Schreiben der Tabellen die Art und Weise der Abfragen mit bedacht wurden.

Cassandra besitzt eine eigene Abfragesprache, die Cassandra Query Language (CQL). CQL ähnelt SQL, besitzt aber einen geringeren Funktionsumfang (kein JOIN, GROUP BY oder FOREIGN KEY). Dies ist unter anderem der Einschränkungen durch die spaltenweise Speicherung der Daten geschuldet und zeigt somit einen Nachteil der spaltenorientierten Speicherstrategie auf.

Cassandra wird unter anderem von Walmart, Apple, Netflix und dem CERN eingesetzt.


![][img-hbase]  

## HBase


HBase ist ein spaltenorientiertes Datenbanksystem zur verteilten Speicherung großer Mengen semistrukturierter Daten und ist in Java implementiert. Bei HBase handelt es sich auch um die Nachmodellierung von Googles BigTable[<sup>2][foot32]. Es setzt auf dem Hadoop Distributed File System (HDFS) auf, somit ist ein Hadoop-Cluster die Voraussetzung für den produktiven Einsatz. Die Replikationsfunktion ermöglicht den Betrieb über mehrere Rechenzentren hinweg.

Die Zugriffe auf die Datenbank können primär über MapReduce[<sup>3][foot33] erfolgen, es ist jedoch auch möglich, höhere Abfragesprachen zu verwenden. HBase lässt sich zum Beispiel mit Pig oder Hive ansprechen, was ein komfortableres Arbeiten zulässt.

Im Gegensatz zu Cassandra lässt sich HBase schlecht über mehrere Datenzentren hinweg verteilen, da die einzelnen Maschinen im Clusterverbund nicht als eigenständige und vollwertige Datenbankknoten agieren. Auch ist HBase langsamer im Schreiben von Datensätzen, da ein zusätzlicher Layer mit Hadoop hinzukommt. (Fasel 2016)

HBase speichert Datensätze in einer Tabelle ab, welche, analog zu Cassandras Keyspace, eindeutige Schlüssel pro Datensatz und Column Families abspeichert. HBase kennt keine Konzepte von Super Columns oder Super Column Families. Tabellen können in Namespaces zusammengefasst werden. Dieses Konzept ist dem Konzept eines Schemas einer relationalen Datenbank.

HBase wird von verschiedenen großen Unternehmen erfolgreich eingesetzt, dazu zählen beispielsweise  Facebook, Twitter, Yahoo!, Adobe, Apple und eBay.


***



<a name="footnote31"></a> <a><sup>1</sup></a> Apache Hadoop ist ein Framework für skalierbare verteilt arbeitende Software. In einem Ein Cluster ist ein Rechnerverbund von vernetzten Computern. 

<a name="footnote32"></a> <a><sup>2</sup></a> Bigtable, Google 2006
http://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf


<a name="footnote33"></a> <a><sup>2</sup></a> MapReduce ist ein vom Unternehmen Google eingeführtes Programmiermodell für nebenläufige Berechnungen über sehr große Datenmengen auf Computerclustern. (Wikipedia.org)


***

Nächstes Kapitel: [4. Key-Value-Datenbanken][kap4]  


[kap4]:             ./4_key_value_db.md "Key-Value-Datenbanken"
[foot31]:	#footnote31
[foot32]:   #footnote32
[foot33]:   #footnote33
[img-cass]:      ./img/cassandra.png "CASSANDRA"
[img-hbase]:      ./img/hbase.png "HBASE"

