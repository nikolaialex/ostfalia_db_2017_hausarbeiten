# Datenbank Anforderungen [7][8]

Im folgenden werden wichtige Anforderungen an die Datenbanken einer IoT-Architektur formuliert. Allen Klassen gemein ist, dass die zu speichernden Daten, wie im vorherigen Kapitel aufgezeigt, in Form von zeitbasierten heterogenen Nutzdaten vorliegen. Daraus ergibt sich die wichtigste Anforderung an ein Datenbanksystem (DBMS) für die IoT-Architektur: die Fähigkeit mit heterogenen Daten umzugehen.

Die zwei bekanntesten Datenbanksystem-Typen sind relationale Datenbanksysteme wie MySQL, Oracle, MsSql, etc. und die schemafreien Datenbanken wie MongoDB, CouchDB, Redis, etc. Während relationale Datenbanksysteme den Fokus auf Konsistenz legen und versuchen dem ACID-Prinzip zu folgen, legen schemafreie Datenbanken den Fokus auf die Verfügbarkeit der Daten und folgen dem BASE-Prinzip.

Um ein geeignetes DBMS für die IoT-Architekturen zu finden, sollten die folgenden Anforderungen beachtet und vom DBMS umgesetzt werden können. Dies kann auch je nach Klasse (Cloud, Gateway, IoT-Gerät, Device) unterschiedlich sein, da jede Klasse die Anforderungen unterschiedlich stark fordert.

## Skalierbarkeit
Die Skalierbarkeit bei einem DBMS ist wichtig, wenn die Last der Datenbank steigt, also weitere IoT-Geräte hinzukommen und massig Daten erzeugen.
Im Bereich der DBMS wird zwischen vertikaler und horizontaler Skalierung unterschieden. Die vertikale Skalierung meint die Erweiterung der Ressourcen eines Datenbank-Servers. Die horizontale Skalierung besteht aus der Erhöhung der Anzahl der Datenbank-Server, welche sich die Datenlast teilen. [12]

In den Bereichen der IoT-Geräte ist die Skalierbarkeit nicht von großem Interesse, da die zu erwarteten Anbindungen bereits bekannt oder durch die Hardware vorgegeben sind und die Daten nicht auf Dauer vorgehalten werden müssen.

Bei dem Gateway kommt es auf die Ausgestaltung an. Je nach Größe des zu verwaltenden Edge-Systems kann eine Skalierung in horizontaler Richtung in Frage kommen, da auch die Systemressourcen der Gateway-Geräte begrenzt sind.

Im Bereich der Cloud sieht dies anderes aus. In der Cloud laufen alle Daten zusammen, somit muss die Datenbank skalierbar sein, um neue Geräte und Edge-Systeme aufnehmen zu können. Ebenfalls greifen die Benutzer mittels Abfragen auf diese Daten zu und erwarten eine schnelle Reaktionszeit des Systems.

## Echtzeit-Datenverarbeitung
Gerade im Bereich der IoT sind Echtzeitdaten-Abfragen, wie zum Beispiel der aktuelle Stromverbrauch sowie der letzten Stunde, sehr relevant und müssen sehr schnell zur Verfügung gestellt werden. Dies betrifft im Grunde alle Klassen, da diese alle ihre Daten in Echtzeit liefern müssen, um diese so schnell wie möglich im Gateway oder der Cloud dem Anwender zur Verfügung stellen zu können. Ebenfalls dienen die Daten für weitere Geräte zur Steuerung. Veraltete Daten sind für eine Prozesssteuerung nicht relevant.

## heterogenen Daten
Ein Device liefert immer nur eine für sich abgestimmte Datenstruktur zurück, somit ist die Fähigkeit mit heterogenen Daten umgehen zu können, für diese Klasse nicht relevant. Ein Thermometer wird immer nur seine eigenen Daten zurückgeben.
Bei einem IoT-Gerät, an dem mehrere Devices kaskadiert werden, ist diese Fähigkeit bereits relevant. Die übermittelten Nutzdaten der einzelnen angeschlossenen Devices müssen so gespeichert werden, dass diese ohne zuvor die Struktur zu kennen, an die weiteren Verarbeitungsschritte weitergegeben werden können.
Ein Gateway für ein Edge-System nimmt wiederum die gesammelten Daten aller angeschlossenen IoT-Geräte auf und besitzt so schon eine große Vielfalt an unterschiedlichen Nutzdatenstrukturen. Diese müssen so gespeichert werden, dass eine Verarbeitung und Weiterleitung erfolgen kann.
In der Cloud laufen schlussendlich alle Daten zusammen. Hier ist die Fähigkeit mit heterogenen Nutzdaten umgehen zu können am stärksten ausgeprägt. Es können jederzeit neue Edge-Systeme oder IoT-Geräte hinzukommen, so dass nicht jedes mal das DBMS angepasst werden muss.
Für alle Klassen soll es Ziel sein, dass nach Möglichkeit bei neuen Nutzdatenstrukturen die Datenbanken nicht angepasst werden müssen.


## Transaktionssicherheit
In traditionellen relationalem DBMS garantierten Transaktions-Management-Mechanismen die ACID-Eigenschaften zur Einhaltung der Gesamtdatenintegrität. Die ACID-Eigenschaften sind Atomarität, Konsistenz, Isolation und Dauerhaftigkeit. Eine Transaktion muss vollständig oder nicht vollständig abgeschlossen sein. Ebenfalls muss eine Transaktion die Datenbank in einem konsistenter Zustand hinterlassen, Transaktionen sollten keine anderen Transaktionen beinhalten, Zwischenergebnisse und Änderungen durch eine Transaktion müssen dauerhaft sein. Die dynamische Beschaffenheit von IoT-Systeme erschwert die Wartung dieser ACID-Eigenschaften. Wenn es sich bei dem IoT-System um ein geschlossenes System handelt (z.B. Hausautomation), dann können diese traditionellen Methoden gelten. Aber für öffentliche IoT-Systeme (z.B. SaaS) müssen diese ACID-Eigenschaften relativiert werden. IoT-Geräte generieren sehr schnell viele unterschiedliche Daten und erzeugen so nicht die gleichen Arten von Transaktionen, die man in traditionellen Bereich vorfindet. Somit ist der Bedarf an ACID-Transaktionen reduziert.
Die Ausführung von verteilten Abfragen basiert auf dem Transparenzprinzip, welches besagt, dass eine Datenbank immer noch als eine große logische zentrale Einheit gesehen wird und die ACID-Eigenschaften durch ein zweistufiges Commit-Protokoll garantiert werden. In einigen Anwendungen werden den Daten der Standort und weitere Kontexte bzw. Metadaten beigefügt. In solchen Fällen darf die Transparenz nicht zwingend erforderlich sein.

Gerade die Replikation der Daten von einem Gateway bzw. eines Edge-Systems in die Cloud sollte nicht nach dem ACID-Prinzip erfolgen, wenn es zu Probleme beim der Übertragung kommt. Primäres Ziel ist, dass die Daten zeitnah ankommen. Sekundäre Ziel ist, dass diese auf allen Servern zeitgleich vorhanden sind. Somit ist ein Verfahren nach dem BASE-Prinzip zu bevorzugen, welches eine grundsätzliche Verfügbarkeit und eine spätere Konsistenz anstrebt. Zu Gute kommt hierbei ebenfalls, dass in IoT-Umgebungen meist nur neue zeitbasierte Daten erzeugt werden und keine bestehenden, alte Daten bearbeitet werden. Eine Änderung eines Temperaturwertes von vor 10 Minuten ist nicht gewünscht, da dies eine Betrachtung der Historie entgegen spräche.

## Hohe Verfügbarkeit
Sollte ein Datenbank-Knoten im Cluster ausfallen, sollte es dennoch möglich sein Lese- und Schreibzugriffe auf die Daten durchzuführen. Die Datenbanken sollten den single point of failure sowie anderweitige Unterbrechungen, wie Hardware- oder Netzwerkausfall, verhindern. Hohe Verfügbarkeit kann durch Redundanz erreicht werden, wie sie in vielen BigData-Datenbanken Anwendung findet. Eine weitere Möglichkeit die hohe Verfügbarkeit zu erhalten sind vorgeschaltete Distributed-Messaging-Systeme wie Apache Kafka. Diese puffern auflaufende Schreiboperationen, wenn die Datenbank diese auf Grund hoher Last oder Ausfall nicht in Echtzeit abarbeiten kann. Sobald die Datenbank wieder einsatzbereit ist, wird die Warteschlange abgearbeitet ohne dass Daten verloren gehen. Die Verfügbarkeit in der Cloud ist somit eine kritische Anforderung an eine dort im Einsatz befindliche Datenbank.
Bei einzelnen IoT-Geräten innerhalb eines Edge-Systems kann auch die Möglichkeit bestehen, dass ein Sensor keine Daten liefert, was aber im Vergleich zur Cloud oder zum Gateway nur marginal auffallen sollte, oder zu einer entsprechenden Ausfall-Warnung im System führt.
Ein Gateway-System besitzt in diesem Zusammenhang eine ähnliche Priorität wie eine Cloud, da dort die Daten zusammenlaufen und je nach Aufgabe Steuerungen oder Analysen stattfinden. Gerade in Hinblick auf die Echtzeitsysteme ist ein Ausfall einer Gateway-Datenbank ebenfalls sehr kritisch zu betrachten.

## Räumlich-zeitliche Skalierbarkeit
Alle Daten, die durch IoT-Geräte erzeugt werden, weisen die Gemeinsamkeit auf, dass diese zeitbasiert sind. Neben der Zeit kommen je nach Anwendungsfall noch weitere Dimensionen, wie die der Räumlichkeit mit dazu. Solche mehrdimensionalen Daten in einem klassischen relationalem DBMS zu verwaltet ist zwar realisierbar, geht aber zumeist zu Lasten der Geschwindigkeit. Die gewählten Systeme sollten somit in der Lage sein, mit solchen mehrdimensionalen Daten umgehen zu können und eine schnelle Filterung auf deren Basis zuzulassen.
Dies gilt insbesondere für die Cloud aber auch für Gateways, je nachdem wie die Edge-Systeme definiert sind.

## Schnell und zuverlässig
Anwender von IoT-Systemen haben unterschiedlichste Anforderungen an die Daten. Während ein Standard-Anwender nur ein paar Informationen über die Geräte haben möchte, benötigt ein Experte die Daten für große Echtzeit-Analysen. Bei den riesigen zu erwartenden Datenmengen ist es sehr unwahrscheinlich, dass diese in einer einheitlichen Struktur vorliegen - zu mindestens, wenn es sich nicht um ein geschlossenes System handelt.
Aktuelle Abfragesprachen wie SQL setzen strukturierte Daten voraus. Halb-strukturierte Daten können mittels XML gespeichert werden, welche mittels XQuery abgefragt werden können.
Somit muss die Abfragesprache des eingesetzten DBMS Anwender-Anfragen schnell und effektiv lösen können.
Bezogen auf die Klassen ist diese Anforderung für alle gültig, da die Daten schnell und zuverlässig erfasst und an die weiteren Klassen übergeben werden müssen.

## Systemreife [8]
Mit dem Begriff der Systemreife ist gemeint, dass ein DBMS soweit entwickelt ist, dass die gängigen Operationen und Schutzmechanismen wie Authentifikation, Datenvertraulichkeit und Intrigrität abgedeckt sind. SQL ist eine ausgereifte Technologie, welche die angesprochenen Funktionen bereist implementiert. Im Bereich der NoSQL Datenbanken sind diese bisher noch nicht vollständig umgesetzt. Sollte ein IoT-Gerät vertrauliche Daten erfassen, so sind die genannten Funktionen unabdingbar und mit sicheren Übertragungswegen zu paaren, so dass ein bestmöglicher Schutz der Daten gewährleistet ist. Gleiches gilt verstärkt für die Gateways und die Cloud, da hier durch Sicherheitsprobleme viele Geräte im Zugriff durch Dritte wären.





Die genannten Anforderungen gelten hauptsächlich für die Datenbank der Cloud und der Gateways für Edge-Systeme. Für die Datenbanken der IoT-Geräte gelten abgeschwächte Anforderungen. Bedingungen, wie das Handling von heterogenen Daten, sind bei einem IoT-Gerät sehr begrenzt, da die Anzahl der angeschlossenen Geräte meist bekannt ist. Ebenfalls gehört das Vorhalten historischer Daten und entsprechende Auswertemöglichkeiten nicht mit zu den Aufgaben eines entsprechenden IoT-Geräts. Innerhalb eine IoT-Geräts können, bedingt durch die begrenzten Ressourcen, nur performante kleine Datenbanken eingesetzt werden. Hierbei werden weniger heterogene Daten verarbeitet, deren Struktur zuvor bekannt ist. Des Weiteren müssen die anfallenden Daten nur so lange vorgehalten werden, bis diese über ein Gateway persistiert wurden.


[Datenstruktur](03_4_datenstruktur.md) | [Vergleich Datenbanken](03_6_vergleich.md)
