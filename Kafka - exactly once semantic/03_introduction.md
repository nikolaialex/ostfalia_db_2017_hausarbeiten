# Einleitung

In einem Unternehmen existieren in den meisten Fällen mehrere Applikationen, die unentwegt Daten in Form von Ereignissen (Events) produzieren. Bei diesen Events kann es sich um Log-Einträge von Webservern, Nutzeraktionen, Nachrichten an externe Systeme oder etwas anderes handeln. Diese Daten werden zwischen den Systemen im Unternehmen ausgetauscht, um beispielsweise Auswertungen durchzuführen oder damit ein anderes System auf bestimmte Ereignisse reagiert. Im einfachsten Fall existieren im Unternehmen nur eine Hand voll Systeme, die direkt untereinander kommunizieren. Diese direkten Kommunikationswege lassen sich bei wenigen Systemen noch gut organisieren. Wenn nun allerdings immer mehr Systeme dazu kommen und nahezu jedes System mit Jedem kommuniziert, erreicht man einen Punkt an dem die Kommunikationswege nicht mehr so einfach zu handhaben sind. Was passiert beispielsweise, wenn ein System eine Downtime benötigt und man erst herausfinden muss, von wie vielen Systemen dieses System entweder Daten empfängt oder an welche Systeme es Daten versendet. Mitunter kann das System auch einfach einen Ausfall haben, und die einzelnen Systeme müssen sich darum kümmern wie sie damit umgehen.

Um dieses Problem zu umgehen, implementiert man zwischen den Systemen eine eigene Anwendung, die sich um den Empfang und die Weitergabe der Nachrichten (Events) aus den einzelnen Systemen kümmert. Die Anwendung abonniert die Daten der Systeme und veröffentlicht sie für andere Systeme, die die Daten nutzen. Das ist dann ein Publish-Subscribe Messaging System. In einem Unternehmen könnten dann mehrere solcher Systeme existieren, die für die Vermittlung von Nachrichten einer bestimmten Sorte verantwortlich sind. Zum Beispiel ein System für die Vermittlung von Logs, Ein System für die Vermittlung von in Anwendungen gesammelten Metriken oder ein System für die Aufzeichnung von Nutzerverhalten. Mit eigenen Systemen für diese Nachrichtenvermittlung entsteht schnell viel doppelte Arbeit, obwohl alle Systeme im Grunde nur Nachrichten über Ereignisse von einem Sender an einen Empfänger vermitteln.

An diesem Punkt kommt Apache Kafka ins Spiel. Apache Kafka ist ein Open Source Software Projekt der Apache Foundation, das ursprünglich bei LinkedIn entwickelt wurde. Apache Kafka wird auf der eigenen Webseite als "distributed streaming platform", also eine verteilte Platform für Datenströme, bezeichnet. Es versteht darunter grob folgende Anwendungsmöglichkeiten:

* Veröffentlichen und Abonnieren eines Datenstroms (Streams) in Form von Ereignissen, ähnlich eines Publish-Subscribe Messaging Systems
* fehler-tolerante Speicherung von Datenströmen
* Verarbeitung von Einträgen in Datenströmen, sobald diese auftreten

Da Kafka im wesentliche ein Message-Broker ist bzw. dies zu den Hauptaufgaben von Kafka gehört, gibt es zwei Seiten, die von Kafka bedient werden. Auf der einen Seite gibt es die Publisher und auf der anderen Seite die Consumer. Kafka erhält Daten von den Publishern, verarbeitet die Daten und publiziert die Daten an die entsprechenden Consumer.
Zu diesem Zweck werden sog. Topics genutzt. Ein Publisher produziert Daten zu einem bestimmten Topic und das Kafka-System publiziert die Daten an alle Konsumer, die sich für dieses Topic angemeldet haben.
Ein weiterer Aspekt ist, dass Kafka sog. Partitionierung nutzt. Dadurch ist ein Broker immer für ein Topic zuständig.

Die folgende Abb. zeigt den o.g. Sachverhalt.  
![Kafka Überblick](/images/kafka_ueberblick.png)

Bei der Verarbeitung von Daten ist noch zu erwähnen, dass Kafka einen sog. Commit Log erstellt. Dieser hilft bei verschiedenen Fällen. Einmal dient er dazu einem Consumer, bspw. nach einem Ausfall,  die verpassten Daten zu übergeben. Zum anderen kann es aber auch dazu dienen nur die aktuellen Daten vorzuhalten. Dazu wird anhand eines Keys überprüft, ob hierzu ältere Daten vorliegen. Ist dies der Fall werden diese gelöscht.

Typische Use-Cases für Kafka sind Messaging, Website Activity, Metrics, Log Aggregation, Stream Processing, Event Sourcing und Commit Logs.

Im Bereich des Messaging entspricht Kafka einem typischem Message Broker wie bspw. ActiveMQ oder RabbitMQ. Kafka wird in diesem Zusammenhang bspw. zum Entkuppeln von Processing und Daten-Produktion oder auch zum Vorhalten von unverarbeiteten Daten genutzt.

Website Activity Tracking war die ursprünglich einzige Aufgabe von Kafka. Daten in einem Publish-Subscribe-System sollten analysiert werden. Hier werden also Daten wie Page Views, Suchen u.ä. anylisert.

Der Metrics-Bereich entspricht dem aggregieren von Statistiken.

Log Aggregating wird genutzt um physikalische Logs von einem oder mehreren Servern in einem zentralen Ort zu verwalten und verarbeiten.

Stream-Processing bedeutet, dass eine Pipeline zwischen Producer, Kafka und Konsument aufgebaut wird. Kafka erhält hier aus verschiedenen Quellen Daten, entfernt bspw. Duplikate und übergibt die Daten an die entsprechenden Konsumenten.

Beim Event Sourcing werden bspw. state changes in einer zeitlichen Sortierung abgelegt. So kann im Nachhinein nachvollzogen werden, wieso ein bestimmter state gesetzt wurde / entstanden ist.

Commit Log wurde bereits zuvor erwähnt. Hier werden Daten aus verschiedenen Quellen vorgehalten und an verschiedene Konsumenten verteilt sowie bei Bedarf ältere Einträge ersetzt oder auch gelöscht.

Neben LinkedIn, wo Kafka als für verschiedene Metriken genutzt wird, wird es auch von vielen weiteren namhaften Unternehmen wie bspw. Netflix. Paypal oder Goldman Sachs genutzt. Netflix nutzt Kafka bspw. zum Real-time monitoring und als event-processing pipeline. 
Insgesamt zeigt sich, dass gerade Unternehmen mit großer Datenverarbeitung (s. bspw. Netflix) Kafka nutzen.
