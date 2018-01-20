# Stream Processing in Kafka

Im Big Data Bereich war es bisher üblich große Mengen an Daten über sogenannten Batch Jobs zu verarbeiten. So wurden die Daten zuerst in großen Mengen über den Tag hinweg gespeichert und dann über Nacht von Anwendungen mit der Hilfe von Apache Hadoop oder ähnlichen Anwendungen in Clustern verteilt verarbeitet, damit am nächsten Tag die aktualisierten Ergebnissen vorliegen konnten. Aufgrund dieser Form der Verarbeitung wirkt sich eine neue Rezension in einem Online-Shop beispielweise erst viel später auf den Durchschnittswert des Produkts aus, obwohl der Autor der Rezension eine sofortige Aktualisierung erwarten würde.

Dieses Problem kann durch Stream Processing gelöst werden. Bevor man jedoch Stream Processing verstehen kann, ist es wichtig zu wissen, was ein solcher Stream ist. Ein Data Stream oder auch Event Stream, also ein Strom von Daten oder Ereignissen, ist eine Abstraktion für einen uneingeschränkten Datensatz. Mit uneingeschränkt ist in diesem Fall gemeint, dass dieser Datensatz ständig neue Daten erhält und nie "komplett" ist. Ein solcher Strom an Daten kann alles Mögliche sein. Es kann sich um Kontotransaktionen, Sensordaten, verschickte E-Mails oder jede andere Form von Ereignissen handeln, die man in irgendeiner Form analysieren könnte.

Beim Stream Processing geht es um die andauernde Verarbeitung eben dieser Datenströme. Es werden ständig neue Ereignisse aus dem Datenstrom ausgelesen, verarbeitet und ausgegeben. Neben den Funktionalitäten für Streams in Kafka existieren eigenständige Stream Processing Frameworks wie Apache Flink oder Apache Storm. [NarkhedeShapiraPalino2017-07]

Kafka unterstützt seit Version 0.10 eine Java API für Streams. Damit integriert Kafka Stream Processing unabhängig von externen Stream Processing Frameworks. Die Verarbeitung erfolgt dabei nicht in Kafka selbst, also nicht innerhalb der Broker, sondern in der eigenen Applikation, die man dafür an Kafka anbindet. Kafka stellt hierbei die Bibliothek für die Verarbeitung zur Verfügung. Die eigene Applikation liest dabei die Daten aus Kafka Topics und schreibt sie nach der Verarbeitung wieder nach Kafka in ein Topic.

So lässt sich beispielsweise eine einfache Anwendung zum Zählen der Wörter in einem Topic schreiben. Ein nützlicherer Anwendungsfall wäre wohl aber die Verarbeitung von Produktbewertungen. So könnten einzelne Bewertungen in einem Topic auf ihre Echtheit geprüft werden, um Fake-Bewertungen zu vermeiden. Zudem könnte ein Durchschnitt über die Bewertungen gebildet werden, den man dann am Produkt anzeigen könnte.

Besonders beim Stream Processing wird die exactly once Semantik wichtig, wenn es darum geht Werte zu aggregieren oder einen Durchschnitt zu bilden. Doppelte oder fehlende Einträge führen hier zu verfälschten Ergebnissen.

Das folgende Code-Beispiel ist für Java 8 und kommt von der offiziellen Dokumentation für Kafka Streams. [kafka-streams] Es wurde für ein besseres Verständnis mit Kommentaren versehen. Es implementiert einen Wortzähler, der aus einem Kafka Topic Textzeilen liest und die darin enthaltenen Wörter gruppiert und zählt. Die Ausgabe wird in ein anderes Kafka Topic geschrieben, in dem danach zu jedem Wort die Anzahl der Vorkommnisse im ursprünglichen Topic steht.

```java
// Hier sehen wir die beschriebenen Bibliotheken, die wir von Kafka importieren müssen
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.StreamsBuilder;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.Topology;
import org.apache.kafka.streams.kstream.Materialized;
import org.apache.kafka.streams.kstream.Produced;
import org.apache.kafka.streams.state.KeyValueStore;

import java.util.Arrays;
import java.util.Properties;

public class WordCountApplication {
  public static void main(final String[] args) throws Exception {
    Properties config = new Properties();
    // Mit der Konfiguration wird definiert, welcher Kafka-Cluster angesprochen wird
    config.put(StreamsConfig.APPLICATION_ID_CONFIG, "wordcount-application");
    // Hier reicht die Angabe eines Brokers um mit dem Cluster verbunden zu sein
    config.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, "kafka-broker1:9092");
    // Die folgenden zwei Zeilen dienen zur Angabe der Serialisierung der Keys und Values aus dem Topic
    config.put(StreamsConfig.DEFAULT_KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass());
    config.put(StreamsConfig.DEFAULT_VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass());

    StreamsBuilder builder = new StreamsBuilder();
    // Hier wird das Topic (TextLinesTopic) angegeben aus dem gelesen wird, also der Input
    KStream<String, String> textLines = builder.stream("TextLinesTopic");
    // Hier findet die Verarbeitung der Daten statt
    // Aus dem Topic kommen einzelne Textzeilen, die in ihre Wörter aufgeteilt werden
    // Das Ergebnis wird nach Wörtern gruppiert und pro Wort wird die Anzahl der Vorkommnisse gezählt
    KTable<String, Long> wordCounts = textLines
      .flatMapValues(textLine -> Arrays.asList(textLine.toLowerCase().split("\\W+")))
      .groupBy((key, word) -> word)
      .count(Materialized.<String, Long, KeyValueStore<Bytes, byte[]>>as("counts-store"));
    // Das Ergebnis wird in ein Topic (WordsWithCountsTopic) zurück nach Kafka geschrieben
    wordCounts.toStream().to("WordsWithCountsTopic", Produced.with(Serdes.String(), Serdes.Long()));
    KafkaStreams streams = new KafkaStreams(builder.build(), config);
    streams.start();
  }
}
```