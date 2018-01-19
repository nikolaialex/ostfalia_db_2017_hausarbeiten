# 9. Fazit

Beim Arbeiten mit Beam stellt man fest, dass es sich um ein mächtiges Werkzeug zum Definieren und Ausführen von Batch- und Streamingdaten-Pipelines handelt. In dem fundamental Aufbau (What, Where, When, How) wurde dargestellt, wie vielseitig Apache Beam einsetzbar ist.

Mithilfe der verschiedenen Runner können unterschiedliche Funktionen des Beam Model genutzt werden. Dabei wurde ebenfalls noch einmal die Mächtigkeit von Apache Beam deutlich. Nicht nur dass das Beam Model alle Anwendungsfälle der Runner abdeckt, bietet das Beam Model sogar weitere Funktionalitäten an, die noch nicht von den Runner genutzt werden wie z. B. die Meta[data] driver triggers und Timer in _When_.

Das Beam Model bietet viele Vorteile: Power, Correctness, Modularity, Flexibility und Composability. Apache Beam ist nach diesem Modell umgesetzt und dadurch sehr flexibel und schnell veränderbar, was den Programmcode betrifft. Das historische Problem, was Stream Processing und die fehlerhafte Berechnung von Daten hervorbringt, kann durch viele Funktionen mit neuen Ansätzen (Accumulating & Retracting) verbessert werden. All diese Funktionen und Einflüsse, die mit Apache Beam einhergehen, bringen eine gewisse Mächtigkeit bzw. Power mit sich.

An dem Beispiel “MinimalWordCount” wurde die Umsetzung einer Pipeline gezeigt und genauer dargestellt. Es können dafür unterschiedliche SDKs und Runner verwendet werden.
Die Beam Vision ist es so viele SDKs und Runner zu Verfügung zu stellen, wie möglich. So kann jeder Entwickler in seiner Lieblingsprogrammiersprache und Lieblingslaufzeitumgebung arbeiten.

Aus dem MapReduce-Google-Paper ist die Idee für das Pipelining und das Zusammenfassen von Batch und Steam-Prozessen geboren. Auf der einen Seite wurden bei Google weiter Paper zu diesem Ansatz veröffentlicht (Flume, Megastore, Millwheel, usw.). Diese Paper haben dann Apache Projekte wie Hadoop, Spark, usw. positiv beeinflusst. Google entwickelte 2014 das Produkt Google Cloud Dataflow, welches den Wissensstand aus den Papern umsetzt. Google überlässt 2016 der Apache Software Foundation den Programmcode, um das Projekt mithilfe der Apache Community zu verbessern und zu verbreiten. Somit war 2016 Apache Beam geboren und am 29.12.2016 wurde die erste nicht incubated Version von Apache Beam veröffentlicht. Aktuell gibt es die Version 2.2.0. Die Beteiligung für das Projekt ist groß und es ist noch vieles für die Zukunft geplant. 

---------

[☜ vorheriges Kapitel](8_Evolution_von_Apache_Beam.md)
   |   [nächstes Kapitel ☞](10_Literaturverzeichnis.md)
