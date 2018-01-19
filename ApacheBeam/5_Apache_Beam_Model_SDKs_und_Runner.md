# 5. Apache Beam: Model, SDKs und Runner

## Beam Model
Apache Beam stellt eine portable API Ebene bereit, um die Pipelines auf unterschiedlichen Plattformen ausspielen zu können. Die Schnittstelle, die Apache Beam bietet basiert auf dem Beam Model. Implementiert wurde die API in den unterschiedlichen Runnern. 
Mit einem Beam SDK wird eine Pipeline definiert. Der kompilierte Programmcode wird dann auf einem unterstützen verteilten Prozess-Backend mit einem Runner ausgeführt z.B. Apache Flink. [**[Beam18a]**](10_Literaturverzeichnis.md)[**[Beam18c]**](10_Literaturverzeichnis.md) 

## SDKs (Software Developer Kits)
Die Beam SDKs unterstützt ein einheitliches Programmiermodell welches ein Datensatz jeglicher Art darstellt und transformieren kann. Sowohl der Input eines begrenzten Datensatzen einer Batch Datenquelle als auch ein endloser Datensatz einer Streaming Datenquelle kann verarbeitet werden. Für die Darstellung und die Transformation der Datensätze werden in den Beam SDKs die gleichen Klassen verwendet ohne zwischen den zwei Arten von Datenquellen zu unterscheiden. 
Aktuell gibt es ein Java und ein Python SDK. 


## Runner
>“A Beam Runner runs a Beam pipeline on a specific (often distributed) data processing system.” [**[Beam18b]**](10_Literaturverzeichnis.md)

Ein Runner übersetzt die Pipeline von dem geschriebenen Programmcode in die kompatible API des gewählten (verteilten) Prozess-Backends. Die Kompatibilität unterschiedlicher Laufzeitumgebung wird durch die abstrakte Ebene (Beam Model) ermöglicht. Dabei werden die unterschiedlichen Funktionen der Laufzeitumgebungen in dem Runner übernommen.  
Aktuell werden die Runner mit folgenden verteilten Prozess-Backends unterstützt: Apache Apex, Apache Flink, Apache Gearpump, Apache Spark, Google Cloud Dataflow. Zusätzlich ist es möglich die Pipeline lokal auszuführen um zu Testen und zu Debuggen.

## Vorteile des Beam Models

Die Genauigkeit (**Correctness**) für Berechnungen im Stream-Processing wird erhöht. Im Abschnitt [How: Accumulating & Retracting](3_Beam_Model.md#how) ist ein Szenario beschrieben, welches diesen Vorteil verdeutlicht.

Die Mächtigkeit (**Power**) die das Beam Model freisetzt, ist ein weiterer Vorteil. Es können viele Möglichkeiten mit geringem Aufwand erschlossen werden. Die Datenverarbeitung kann z.B. von einem Session-Window schnell von einem festen Zeitfenster im Programmcode zu einem flexiblen abstandsabhängigen Zeitfenster verändert werden, vgl. [Apache Beam Vielfalt](4_Apache_Beams_Vielfalt.md).

Die Kom­pos­si­bi­li­tät bzw. Zusammensetzbarkeit (**Composability**) wird durch die zustandslose und modulare Programmierung gewährleistet. Die ermöglicht es die Komponenten bzw. Funktionen von Apache Beam beliebig oft zu verwenden. Das What, Where, When und How können innerhalb einer Pipeline mehrfach angewendet werden.

Die Flexibilität (**Flexibility**) zwischen Batch und Stream-Prozessen zu wechseln und diese weiter zu beeinflussen ist ein große Vorteil von Apache Beam. Es gibt viele verschiedene Möglichkeiten den Output zu generieren. Einige Beispiele, die Beam bietet: Klassisches Batch-Processing, Batch-Processing mit festen Zeitfenstern, Stream-Processing, Stream-Processing mit spekulativen und verspäteten Daten, Stream-Processing mit Widerruf (Retraction), Session usw.. 

Die Modularität (**Modularity**) bzw. Baukastenprinzip, was Apache Beam verwendet bietet den Vorteil, dass der Programmcode nicht groß verändert oder angepasst werden muss um mehr Funktion einzubauen. Der Code bleibt lesbar und ist gut wartbar. Zusätzlich kann eine Pipeline mehrfach in unterschiedlichen Szenarien verwendet werden.

[**[Yout18a]**](10_Literaturverzeichnis.md) 

---------

[☜ vorheriges Kapitel](4_Apache_Beams_Vielfalt.md)
   |   [nächstes Kapitel ☞](6_Implementierung_Minimal_Word_Count.md)
