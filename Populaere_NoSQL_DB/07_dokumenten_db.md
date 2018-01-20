# 5 Dokumentendatenbanken

Dokumentendatenbanken vereinigen die Schemafreiheit von Schlüssel-Wert-Speichern mit der Möglichkeit zur Strukturierung der gespeicherten Daten (Meier 2016). Dokumente sind strukturierte Daten, beispielsweise im JSON-Format (JavaScript Object Notation), und enthalten Datenstrukturen in der Form von rekursiv verschachtelten Attribut-Wert-Paaren ohne referenzielle Integrität. 

Dokumentendatenbanken sind völlig schemafrei, das heisst, es ist nicht notwendig, vor dem Einfügen von Datenstrukturen ein Schema zu definieren. Das heisst, der Aufbau von Dokumenten muss vor der Speicherung nicht bekannt sein.

Verbreitete Dokumentendatenbanken sind z.B. MongoDB, CouchDB, Couchbase, Firebase Realtime Database, RethinkDB und Cloudant.

![][img-mongo]  

## MongoDB

Der Name der Datenbank ist abgeleitet vom englischen humongous für „extrem groß“. So ist das Ziel des Systems die Speicherung gigantischer Datenmengen bei dennoch hoher Leistung, d.h. sehr kurze Reaktionszeiten von Abfragen. Das System enthält eine Implementierung des Map/Reduce Algorithmus zur verteilten Ausführung von Auswertungen. MongoDB speichert Dokumente im BSON-Format, der binären Repräsentation von JSON mit einigen zusätzlichen Datentypen.

Ein MongoDB Server (später auch als mongod-Prozess bezeichnet) kann mehrere Datenbanken verwalten. Innerhalb der Datenbank werden Dokumente, einzelne Datensätze, in sogenannten Collections organisiert. Sie müssen nicht die gleichStruktur haben und ein Schema muss primär nicht vorgegeben werden. Ein Dokument besteht aus einer geordneten Menge von Feldern, ein Feld wiederum besteht aus einem Key/Value-Paar. Während der Key ein String ist, können die Werte einfache Datentypen, Strings, ein Array oder auch wieder ein Dokument sein.

MongoDB bietet eine Vielzahl von API’s für verschiedene Sprachen wie Java, Python, etc.

Der Replikationsmechanismus von MongoDB funktioniert nach dem Master-N-Slaves-Prinzip, wobei ein Master seine Daten an einen oder mehrere Slaves repliziert. Shards sind separate Datenknoten des MongoDB-Clusters.

![][img-couch]  

## CouchDB

Der Name CouchDB ist die halbironische Abkürzung für „Cluster of unreliable commodity hardware Data Base“. (deutsch: „Datenbank auf einem Cluster aus unzuverlässiger Standardhardware“.) CouchDB ist ein dokumentenorientiertes, schemaloses Datenbanksystem und speichert seine Dokumente im JSON-Format. Jedem Dokument wird ein eindeutiger Schlüssel zugewiesen.

Die Verwaltung von konkurrierenden Zugriffen erfolgt über Versionierung. Durch den Multi-Version Concurrency Control Mechanismus werden Schreib-Lese-Blockaden vermieden; bei Änderungen werden die Daten nicht überschrieben, sondern immer neue Versionen angehängt. Diese werden bei der Replikation nicht mit übergeben. Der Zugriff auf die Daten ist über eine REST-Schnittstelle[<sup>1][foot51] möglich.

Mit CouchDB kann eine gefilterte Abfrage über MapReduce parallel ausgeführt werden.
CouchDB wird aufgrund der oben genannten Eigenschaften vorwiegend für Webanwendungen verwendet, da hier oft viel Text mit einer nicht definierten Länge gespeichert werden muss. Typische Anwendungsbeispiele sind Blogs, Foren, oder Content-Management-Systeme.

***

<a name="footnote51"></a> <a><sup>1</sup></a> REST: Representational State Transfer Protocol: Programmierparadigma für verteilte Systeme, insbesondere für Webservices mit den 6 Prinzipien Client-Server, Zustandslosigkeit, Caching, Einheitliche Schnittstelle, Mehrschichtige Systeme, Code on Demand. Eine saubere REST-Implementierung verlässt sich für Datenbankoperationen auf die in dem HTTP-Protokoll definierten Zugriffsmethoden CREATE (PUT), READ (GET), UPDATE (POST), DELETE , ingesamt CRUD genannt.

***

Nächstes Kapitel: [6. Graphdatenbanken][kap6]  

[kap6]:             ./6_graph_db.md "Graphdatenbanken"
[foot51]:	#footnote51

[img-couch]:      ./img/couch.png "CouchDB"
[img-mongo]:      ./img/mongo.png "MongoDB"


