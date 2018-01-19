# 2 Dokumentenorientierte Datenbanken

Bei dokumentenorientierten Datenbanken werden Daten in Form von Dokumenten gespeichert. Diese benötigen keine vordefinierte Struktur oder festgelegte Datentypen und die Feldlänge muss nicht vorab bestimmt sein. Auf oberster Ebene besteht ein Dokument aus einer Map und somit aus Key-Value-Paaren. Die Keys müssen dabei immer aus Zeichenketten bestehen, während die Values primitive Datentypen, jedoch auch Container-Objekte beinhalten können. Dabei ist die Anzahl der Attribute und deren Verschachtelung beliebig. Die Datenbank selbst ist für das Erkennen von Struktur und Typen verantwortlich. Dokumente besitzen weiterhin keine Relationen zu anderen Dokumenten. Durch den hierarischen Aufbau ist es möglich, zusammengehörige Daten in einem Dokument darzustellen.
Dokumente können auf verschiedenen Servern verteilt liegen und existieren je nach Anwendungsfall durch nicht synchronisierte Änderungen auch in verschiedenen Versionen.

Anwendungsgebiete für Dokumentenorientierte Datenbanken sind Logging zur Echtzeitanalyse, aber auch als Cache im mobilen Bereich, wobei die Responses direkt als Dokument gespeichert und abgerufen werden können.
Dokumentenorientierte Datenbanken sind mehr auf Informationsermittlung und -anzeige spezialisiert. Deswegen ist es empfehlenswert, sie für Use-Cases zu verwenden, bei denen wenig geschrieben und häufig gelesen wird. Über den gesamten Objektgraphen muss die Indexierung erfolgen. Dazu müssen diese denormalisiert werden. Das führt zu einer Performancesteigerung, da keine Joins durchgeführt werden müssen, durch die Redundanz jedoch auch zu erhöhtem Speicherbedarf. Für eine Änderung muss diese nun an mehreren Stellen stattfinden, was die Performance von Schreibzugriffen - besonders im Hinblick auf Konsistenz über alle Dateien - mindert.
Abfragen können direkt über die ID oder den Index stattfinden. Alternativ gibt es auch Ansätze über MapReduce oder einen Volltext Index.

Mit dem Programmiermodell MapReduce ist es möglich, große unstrukturierte bzw. semi strukturierte Datensätze zu verarbeiten. Dabei werden die Daten in Blöcken gespeichert, was sowohl das verteilte Speichern auf mehrere Rechner Einheiten unterstützt, sowie die Aufbereitungszeit durch parallele Verarbeitung verringert. Unstrukturierte Daten werden eingelesen und anschließend deren Inhalt zu Positionen zugeordnet. Jede Zeile wird dabei mittels eines Byte-Offsets identifiziert. Nach einer Transformation der Key-Value-Paare in intermediate Key-Value-Paare, werden diese gruppiert. Dabei werden je Key die Values zusammengefasst, sodass für jeden Key nur noch ein Value vorhanden ist.

## CouchDB

CouchDB wurde von *Lotus Notes* inspiriert und befindet sich seit 2005 als Open Source Projekt unter Leitung der *Apache Software Foundation* in Entwicklung. Implementierungssprache ist Erlang. Das Besondere an dieser Datenbank ist, dass sie nicht nur auf Windows, Linux und OS X lauffähig ist, sondern auch auf den mobilen Betriebssystemen Android und iOS. Die aktuelle Version von November 2017 ist die 2.1.1. Sie kann unter der Apache-Lizenz Version 2.0 benutzt werden. Installiert wird CouchDB über Paketquellen, zum Beispiel von der offiziellen Website. Dabei werden auch die benötigten Erlang-Pakete als Abhängigkeit mit installiert.

### Mehrbenutzerbetrieb
Konsistenz, Verfügbarkeit (User können gleichzeitig auf Versionen von Daten zugreifen) und Partitionstoleranz (über mehrere Server verteilt) gleichzeitig zu erfüllen ist laut CAP Theorem nicht möglich. Im Mehrbenutzerbetrieb ist bei CouchDB die Konsistenz als *eventual Consistency* umgesetzt, d.h. es wird aus Performancegründen bei Schreiboperationen auf die Garantie von Konsistenz verzichtet, um Verfügbarkeit und Partitionstoleranz zu gewährleisten. Die Daten werden nach Beenden der Schreiboperation konsistent gespeichert, nur der Zeitraum ist dabei nicht festgelegt, so dass User zum gleichen Zeitpunkt auf unterschiedliche Versionen von Dokumenten zugreifen.

Im Gegensatz zum Ansatz von relationalen Datenbanken, einen Locking-Mechanismus zu verwenden, wird bei CouchDB Multi-Version Concurrency Control genutzt. Dabei wird kein Locking verwendet, sondern bei Änderungen von Werten in Dokumenten, wird von diesen Dokumenten eine neue Version erstellt. Dadurch können Abfragen parallel gestellt und Antwortzeiten verringert werden. Wenn User 1 einen Request für Dokument A stellt, erhält er als Antwort den zum Zeitpunkt der Abfrage aktuellen Stand des Dokuments. Wenn während der Verwendung von Dokument A ein User 2 einen Request für das gleiche Dokument erstellt, welche dieses ändert, wird eine neue Version des Dokuments erstellt. Somit kann User 1 weiterhin das Dokument A betrachten und im Falle, das ein User 3 das Dokument anfragt, bekommt er die neuere Version als Antwort.

### Performance
Da CouchDB in Erlang geschrieben ist, wird eine vertikale Skalierbarkeit ermöglicht und kann somit auf leistungsfähigen Systemen bis hin zu Geräten mit geringer Ausstattung ausgeführt werden. Weiterhin kann CouchDB nativ als Cluster betrieben werden und anstelle von vielen Servern ein Cluster an Nodes anlegen. Diese sorgen dafür, dass Daten verfügbar sind und nach einer Transaktion dauerhaft gespeichert werden. Des Weiteren unterstützt CouchDB Mehrwege-Replikation. Dadurch ist es möglich, dass Daten von einer Datenbank in eine andere Datenbank auf einem anderen Server repliziert werden. Wenn währenddessen ein Fehler auftritt bzw. ein Konflikt entsteht, wird die Replikation gestoppt und kann händisch behoben werden. Der Vorgang des Replizierens ist inkrementell. Wenn zum Beispiel ein Fehler im Vorgang auftritt, wie eine Trennung der Netzwerkverbindung, wird dieser Vorgang beendet. Sobald der Fehler behoben ist und wieder eine Verbindung besteht, wird der Vorgang nahtlos an der beendeten Stelle fortgesetzt. Dabei findet nur eine Übertragung der Daten statt, die benötigt werden, um eine Synchronisierung zwischen den Datenbanken durchzuführen.

Bei erhöhtem Lastaufkommen, wie etwa viele Requests, erhöht sich bei CouchDB die Latenzzeit. Somit werden um Stabilität zu gewährleisten, weiterhin alle Anfragen beantwortet, dafür werden jedoch Performance Einbußen gebilligt. Caching, abgesehen von Webstandards, ist nicht vorgesehen. Es muss vorher bedacht werden, welche Daten in welcher Form abgefragt werden können und diese werden dann abgespeichert und indexiert.

### Sicherheit
Um Sicherheitsaspekte beim Nutzen der Datenbank zu berücksichtigen, können neben der Standardeinstellung, nach welcher alle User Administratoren sind, auch User mit weniger Rechten angelegt werden. Administratoren können Dokumente lesen, erstellen, löschen und editieren, sowie die Datenbank konfigurieren und weitere Nutzer anlegen. User selbst werden ebenso in Dokumenten gespeichert und Passwörter gehashed. Authentifizierung kann über Cookies stattfinden, sodass Logins über HTML-Formulare gesendet und ein einmaliger Token für einen Client generiert werden. Bei Veränderung eines Dokumentes wird eine Validierungsfunktion in Javascript ausgeführt. Dabei wird eine Kopie des originalen sowie des veränderten Dokumentes übergeben, sowie die Authentifizierung, um festzustellen, ob es dem Nutzer erlaubt ist, das Dokument zu verändern.

### Weitere Tools
Für den Zugriff auf CouchDB existieren verschiedene User Interfaces. Ein oft verwendetes ist *Fauxton*, welches bei den meisten Installationen mitgeliefert wird. Damit ist es möglich, Dokumente zu erstellen, zu bearbeiten und zu löschen, Usermanagement durchzuführen, sowie MapReduce Views zu erzeugen und auszuführen.

## Couchbase
Couchbase ist ein von *CouchDB* und *Membase* abgeleitetes Datenbanksystem. Es wird von *Couchbase, Inc.* unter der Apache 2 Lizenz seit 2011 entwickelt und ist in der aktuellen Version 5.0 von Oktober 2017 verfügbar. Die Implementierungssprachen sind C, C++, Erlang und Go und kann ausgeführt werden auf den Betriebssystemen Linux, Windows, OS X, sowie auf den mobilen Betriebssystemen Android und iOS. Persistenz, Replikation und Sharding sind von Membase übernommen wurden, während von CouchDB die REST Api sowie Speichermechanismen für JSON Dokumente implementiert sind. Dieses Zusammenspiel von Techniken ist auch für verteilte Softwaresysteme geeignet. 

Von Couchbase existieren zwei verschiedene Versionen, eine Community und eine Enterprise Version. Die Community Version kann für nicht kommerzielle Zwecke eingesetzt werden, während die Enterprise Version für kommerzielle Zwecke vorgesehen ist. Laut Lizenz ist es jedoch auch erlaubt, die durch die Community supportete Version für produktive Systeme einzusetzen. Technischen Support, sowie höhere Performance und bessere Sicherheitsfunktionen und Verfügbarkeit der Datenbank ist nur in der Enterprise Edition mit abgeschlossenem Abo enthalten. Für neue und Test-Applikationen ist diese jedoch ebenso kostenfrei. Desweiteren erhält die Community Edition Aktualisierungen deutlich später.

### Mehrbenutzerbetrieb
Ebenso wie bei CouchDB muss das CAP Theorem bedacht werden, im Falle von Couchbase wird jedoch Priorität auf die Konsistenz der Daten gelegt und dafür werden Abstriche hinsichtlich der Verfügbarkeit in Kauf genommen. Clients bekommen Schreibzugriffe anderer Clients mit, da ein solcher Zugriff im Cluster an einen Knoten weitergeleitet wird, der die angefragten Daten zum Zeitpunkt der Abfrage vorhält. Parallele Schreiboperationen auf ein Dokument sind nicht möglich, dafür ist für Transaktionen auf ein Dokument die Atomarität gewährleistet. Durch einen Compare and Swap Mechanismus wird diese Art des Mehrbenutzerbetriebs auf Dokumente sichergestellt. Ein Dokument enthält in den Metadaten eine Versionsnummer, welche bei Änderungen angepasst wird. Schreibzugriffe auf Dokumente mit älterer Versionsnummer sind nicht erlaubt.

### Performance
Zur Skalierung steht Couchbase ein Multi-Dimensionales Skalierungssystem zur Verfügung. Dabei werden Datenbankabfragen, Indizes und Daten voneinander getrennt, so dass die benötigte Hardware unabhängig voneinander optimiert werden kann. Neu dem Couchbase Cluster hinzugefügte Knoten, können weitere Dienste erhalten. Das ist auch im Nachhinein im Livebetrieb möglich. Der Query Service für Datenbankabfragen kann somit zum Beispiel auf abgetrennten Instanzen ausgelagert werden, um eine Blockierung durch zu hohe CPU-Last zu verhindern. Indizes, welche schnelle Festplatten benötigen, können ebenso ausgelagert werden, um somit bei den anderen Instanzen an Ausgaben für schnellen Speicher zu sparen. Weiterhin wird ein Teil der Daten inklusive Indizes in den Hauptspeicher von Knoten ausgelagert. Für die ausgelagerten Systeme lassen sich außerdem Nutzungsobergrenzen angeben.

### Sicherheit
Couchbase bietet standardmäßig Sicherheitsfunktionen für Authentifizierung, Autorisierung und Verschlüsselung an. Authentifiziert werden Administratoren über Passwort oder *Lightweight Directory Access Protocol* (LDAP) und Nutzer nur über Passwörter. Über das Protokoll LDAP kann überprüft werden, ob der Client, welcher einen Request ausführt, dafür die Befugnis besitzt. Somit ist dem Clienten nur ermöglicht Einsicht in die Container zu nehmen, für die ihm die Rechte übertragen wurden. Damit wird gleichzeitig die Autorisierung abgedeckt, womit sich granular festlegen lässt, wie Clienten auf Ressourcen, vordefinierte Aktionen durchführen können. Weiterhin gibt es Möglichkeiten unter Couchbase, Verschlüsselung auf Dateiebene und bei der Datenübertragung einzusetzen. Auf Dateiebene ist von Couchbase selbst keine Verschlüsselung vorgesehen, die Daten liegen demnach unverschlüsselt vor, kann jedoch über Drittanbieter realisiert werden. Zur Datenübertragung zwischen Server und Client wird SSL/TLS Verschlüsselung genutzt. Zur Kommunikation zwischen den Rechenzentren wird TLS eingesetzt.

### Weitere Tools
Mit Couchbase Query Workbench steht dem Nutzer eine GUI zur Verfügung, mit welchem sich Abfragen in der SQL ähnlichen Abfragesprache N1QL formulieren und ausführen lassen können. Die Ergebnisse lassen sich anschließend in JSON, Tabelle oder Baumstruktur darstellen. Weiterhin kann ein Container und dessen Struktur der Dokumente angezeigt werden.

### Beispiel Mobile Nutzung
Ein mittlerweile häufiger Anwendungsfall ist es, Datenbanken auf mobilen Endgeräten zu nutzen. Couchbase ist bekannt dafür, eben diesen Anwendungsfall abzudecken. Folgend sollen kurz die Schritte skizziert werden, um Couchbase auf einem Android Gerät als Entwickler nutzbar zu machen und die Vorteile aufzeigen.
Unter Android lässt sich Couchbase in einer "lite" Variante nutzen. Dazu muss die Library mit einer Zeile Gradle als dependency hinzugefügt werden und anschließend lassen sich, entwickler freundlich ohne Konfigurationsaufwand, Daten in Form von Dokumenten speichern.
```Java
compile 'com.couchbase.lite:couchbase-lite-android:1.4.1'
```
Dazu muss ein Manager Objekt erstellt werden, mithilfe dessen sich eine Datenbank kreieren oder falls schon existent sich diese öffnen lässt. Aus einer Map lässt sich anschließend ein Dokument Objekt erstellen, welches in die Datenbank gespeichert wird.
```Java
Manager manager = null;
try {
    manager = new Manager(new AndroidContext(getApplicationContext()), Manager.DEFAULT_OPTIONS);
} catch (IOException e) {
    e.printStackTrace();
}
// Create or open the database named app
Database database = null;
try {
    database = manager.getDatabase("app");
} catch (CouchbaseLiteException e) {
    e.printStackTrace();
}
// The properties that will be saved on the document
Map<String, Object> properties = new HashMap<String, Object>();
properties.put("title", "Übersicht über populäre NOSQL-Datenbanken");
properties.put("subtitle", "Beispiel mobile Nutzung");
// Create a new document
Document document = database.createDocument();
// Save the document to the database
try {
    document.putProperties(properties);
} catch (CouchbaseLiteException e) {
    e.printStackTrace();
}
```
Das erspart dem Entwickler dahingehend Entwicklungsaufwand, dass nicht für jedes zu speichernde Objekt Tabellen erstellt werden müssen. Besonders wenn die App zum Beispiel in den Hintergrund gelegt wird, müssen Daten gesondert gesichert werden, damit diese bei der Rückkehr wieder angezeigt werden können. Da Couchbase unstrukturierte bzw. semistrukturierte Dokumente zulässt, ist dies mit nahezu allen Objekten ohne größeren Aufwand möglich.
Weiterhin lassen sich so auch Responses von Servern Cachen, ohne dass für jedes Objekt Tabellen oder Relationen erstellt werden müssen, indem zum Beispiel eine JSON als Dokument in der Datenbank gesichert wird und im offline Modus daraus wieder gelesen werden kann.
## MongoDB
MongoDB ist ein Open Source Projekt und befindet sich seit 2009 durch *MongoDB, Inc* in Entwicklung. Geschrieben in der Programmiersprache C++ ist sie lauffähig unter Linux, OS X, Solaris und Windows. Die aktuelle Version von Dezember 2017 ist die 3.6.0. Ziel der Entwicklung ist es, eine möglichst geringe Latenz besonders bei der Verarbeitung großer Datenmengen zu erreichen. 

Wie die meisten dokumentenorientierten Datenbanken ist MongoDB ebenso schemafrei und es muss keine Struktur der Daten vorhanden sein. Gespeichert werden Daten im BSON Format, was eine Erweiterung des JSON Formats ist. BSON nutzt im Gegensatz zu JSON Datentypen, was zwar zu einer höheren Dateigröße führen kann, die Effizienz jedoch erhöht. Weiterhin ist es besser traversierbar. BSON sind hier auf 4 MB beschränkt, was dazu führt, dass zum Beispiel Multimediadateien auf mehrere BSON Dateien verteilt werden. Dies hat unter anderem beim Video Streaming Vorteile, da beim Spulen nicht dass ganze Video geladen werden muss, sondern nur die jeweiligen 4 MB Dateien. Durch *MMAP* wird eine besonders hohe Performance bei Lese- und Schreibzugriffen erreicht. Dabei wird die gesamte Datenbank in den Arbeitsspeicher geladen. Bei Hardware Problemen kann es dabei jedoch zum Verlust der letzten Schreibvorgänge kommen. MongoDB besitzt eine eigene Implementierung von MapReduce, um eine hohe horizontale Skalierbarkeit zu gewährleisten.

Es existieren verschiedene Treiber für MongoDB, darunter Offizielle Treiber, Community Driver und Next Generation Driver, welche unter verschiedenen Lizenzen fallen und unter anderem unterschiedliche Programmiersprachen unterstützen. Als unterstützte Betriebssysteme werden verschiedene Linux Distributionen genannt und Windows Server, jeweils in den 64-bit Versionen. Es wird empfohlen MongoDB in einer virtuellen Maschine auszuführen, da versucht wird, soviel RAM wie möglich zu reservieren.

### Mehrbenutzerbetrieb
Bezüglich Mehrbenutzerbetrieb existieren mehrere Einstellungen um gleichzeitige Datenveränderungen durchzuführen. Diese können je nach Bedürfnis von Performance und Konsistenz angepasst werden. Prinzipiell ist es das Ziel, dass Nutzer zur gleichen Zeit Lese- und Schreibzugriffe durchführen können. Dennoch ist es vorgesehen, die Daten konsistent zu halten. Um das zu erreichen werden multigranulare Sperren genutzt. Dadurch ist der Nutzer in der Lage, Operationen auf Globalem, Datenbank- oder Kollektionslevel zu sperren. Des Weiteren ist ein Lock durch angepasste Speicher Engines mit eigener Concurrency Control auf Kollektionslevel möglich. Seit MongoDB Version 3.2 ist *WiredTiger* die standardmäßig ausgewählte Speichermanagement Engine. Diese bietet Concurrency Control auf Dokumenten Level.

### Performance
Dass MongoDB mit dem Hauptgedanken an Performance entwickelt wurde, ist an direkten Vergleichen mit klassischen SQL-Datenbanken zu sehen. Dabei werden signifikante Unterschiede in der Geschwindigkeit in bestimmten Anwendungsszenarien sichtbar. Drei Skalierungsarten sind für die Steigerung der Performance besonders zu beachten. Cluster Skalierung, Performance Skalierung und Daten Skalierung sorgen dafür, dass große Datenmengen auf eine große Anzahl von Datenbanken verteilt werden, welche wieder auf vielen Clustern laufen. Da viel Wert auf die horizontale Skalierung gelegt wird, ist es sinnvoll, mehrere kleine Server zu verwenden, anstatt einige wenige starke Server. Zu den Best Practices zählt es, häufig benötigte Daten und die Indizes so zu halten, dass diese in den RAM geladen werden können. Wie schon beschrieben, ist mehr RAM der größte Vorteil für Performanceoptimierung.

### Sicherheit
In der Standardeinstellung wird MongoDB ohne Benutzernamen und Passwort installiert. Es werden jedoch Möglichkeiten zum Sichern der Datenbank angeboten. Somit kann zur Authorisierung ein Benutzername und Passwort oder ein Zertifikat verlangt werden. Weiterhin können Nutzern verschiedene vorgefertigte oder selbst definierte Rollen zugewiesen werden, um Zugriffsrechte zu definieren beziehungsweise einzuschränken. Die Kommunikation zwischen Client und Server wird mit Hilfe von OpenSSL mit SSL/TLS verschlüsselt. Die Enterprise Edition bietet außerdem eine Verschlüsselung der gesamten Datenbank an. 

Auch wenn MongoDB eine NOSQL Datenbank ist, wird zur Datenmanipulation eine SQL ähnliche Syntax verwendet, was einen Umstieg von klassischen Datenbanken erleichtert. Außerdem werden dadurch auch Anpassungen an Clients weniger aufwendig, da die Anfragen ähnlich durchzuführen sind.

## Elasticsearch
Elasticsearch wird seit 2010 von *Elastic* entwickelt und basiert auf Apache Lucene. Elasticsearch ist keine Datenbank im klassischen Sinn, sondern ist als Such- und Analyseplattform konzipiert wurden. Der Programmcode ist Open Source, in Java geschrieben und läuft auf allen Betriebssystemen mit einer Java VM. Die aktuelle Version von Dezember 2017 ist die 6.1.1.

Ein Elasticsearch Server lässt sich zum Beispiel über Docker innerhalb von weniger als 10 Sekunden installieren. Konfiguriert wird über Dateien im YAML-Format. *Elastic* spricht anstatt von "schemafrei" von "schema flexibel". Es wird kein vorgegebenes Schema benötigt, jedoch kann eines angegeben werden, um die Performance zu erhöhen. Alternativ kann ElasticSearch auch ein Schema per Heuristik anhand der übergebenen Daten ableiten.

Gespeichert, aktualisiert und indiziert werden Dokumente im JSON Format mithilfe von HTTP-PUT oder HTTP-POST Befehlen. Eine Response gibt anschließend den Status des Aufrufes zurück. Das Lesen findet mithilfe des HTTP-GET Befehls statt und gelöscht werden Dokumente mit dem HTTP-DELETE Befehl.
Bezüglich Relationen und Constraints verhält sich Elasticsearch wie die meisten Dokumentenorientierten Datenbanken, optimiert auf Suche und Informationsermittlung.
Für Suchanfragen, also Datenbankabfragen, steht eine umfassende Query-DSL zur Verfügung. Diese ermöglicht einfache Abfragen nach einem Term in einem bestimmten Feld. Es sind jedoch auch komplexe Queries möglich, die mit boolschen Operatoren verbunden sind. Diese Abfragen werden mit einem HTTP-GET Request durchgeführt, bei welchem ein Argument im JSON-Format angehängt wird.

### Performance
Besonders für Logs bietet sich ElasticSearch an, wenn auf bestimmte datenbanktypische Eigenschaften verzichtet werden kann und Geschwindigkeit eine hohe Priorität bei den Anforderungen ist. Logs werden einmal geschrieben, updates sind normalerweise nicht notwendig, ebensowenig Integrität und Transaktionen. Ein weiterer Vorteil ist die gute Suchfunktion, welche Autocompletion und Rechtschreibprüfung bzw. auch unscharfe Suchen ermöglicht. Weiterhin ist es auch möglich, ElasticSearch vor eine "richtige" Datenbank zu schalten, um die Vorteile beider Systeme zu nutzen.

### Sicherheit
Hinsichtlich Sicherheitsfunktionen kann ElasticSearch nicht mit klassischen Datenbanksystemen konkurrieren. Es existieren keine Funktionen für Autorisierung und Authentifizierung. Damit ist es notwendig, auf Anwendungsebene für die benötigten Sicherheitsfunktionen zu sorgen und Benutzerverwaltung beziehungsweise granulare Berechtigungen zusätzlich zu implementieren.

### User Interface
Mit *Kibana* existiert außerdem ein im Vergleich zu anderen Dokumentenorientierten Datenbanken ein mächtiges User Interface, welches die Auswertung und deren Anzeige ermöglicht beziehungsweise aufwerten kann. Dazu zählen klassische Diagramme wie Tortendiagramm und Histogramme, aber auch Geolocations auf einer Karte oder Zeitachsen.
