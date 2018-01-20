# 4 Key-Value-Datenbanken

Key-Value-Datenbanken besitzen primär ein sehr einfaches Datenmodell, da sie lediglich Schlüssel-Werte-Paare in einer Datenbank ablegen. Die Daten werden zumeist im Hauptspeicher gehalten, man nennt deswegen solche Datenbanken auch In-Memory-Datenbanken. Ein grundlegendes Prinzip von Key-Value-Stores ist, dass der Schlüssel eineindeutig ist. Es werden keine Indizes aufgebaut und die Datenstruktur bleibt eine simple Schlüssel Werte-Paar-Struktur.

Die Architektur ist auf Skalierung ausgelegt; da alle Daten im Hauptspeicher gehalten werden, lassen sie sich sehr schnell abfragen. Deshalb werden sie häufig zum Caching eingesetzt. Sie können auch zur Speicherung komplexer Datenstrukturen direkt aus Anwendungen heraus verwendet werden, denn die Werte können auch komplexe Datentypen bis hin zu serialisierten Objekten sein.

Typische Vertreter sind Redis, Memcached, Hazelcast, Riak KV, Ehcache und Aerospike. Im Folgenden sollen Redis und Memcached näher betrachtet werden.



![][img-redis]  

## Redis

Redis (Remote Dictionary Server) speichert alle Daten im RAM und synchronisiert sich von Zeit zu Zeit auf die Festplatte. Alternativ kann auch etwas langsamer im Append Only Mode alles auf die Festplatte geschrieben werden. Der Fokus liegt auf der Durchführung schneller Schreib- bzw. Leseoperationen und der platzsparenden Speicherung einfacher Datenstrukturen.

Als Datentyp wird ein binary safe String verwendet. Somit können Inhalte eine einfache Zeichenkette oder auch binären Objekte sein (beispielsweise Bilddaten). Die Größe von Key und Value kann maximal 512 MB betragen. Da die Datenstrukturen aber auch Hashes, Listen, Sets und geordnete Sets beinhalten können, hat Redis auch etwas Ähnlichkeit mit Column Stores. Zudem gibt es einige Sortier- und Mengenoperationen. 

Ein weiterer Vorteil von Redis ist, dass Objekte, welche sich schon im Speicher befinden, manipulierbar sind.

Redis ermöglicht eine einfache Master-N-Slaves-Replikation, d.h. es kann nur einen Master geben, an den aber beliebig viele Slaves – in Reihe und in Serie – angebunden werden können. Aufgrund der Geschwindigkeit und Benutzerfreundlichkeit ist Redis eine beliebte Wahl für Web-, Mobil-, Spiele-, Ad-Tech- und IoT -Anwendungen, die eine bestmögliche Leistung erfordern.

Redis wird auch in großen Unternehmen wie Twitter und Amazon eingesetzt.


![][img-mem]  

## Memcached

Memcached wird auch als Cache-Server bezeichnet und dient zur vorübergehender Zwischenspeicherung von Daten aus Datenbanksystemen im Arbeitsspeicher. Memcached wird oft bei dynamischer Erzeugung von Internetseiten benutzt, die Datenbankanbindungen benötigen. Werden Daten von Aufrufen bestimmter Abfragen mehrfach hintereinander benötigt, ist es sinnvoll, diese im Zwischenspeicher und somit schnell verfügbar zu halten.
 (Wikipedia.org)

Die Verbindung zu einem solchen Server findet über die Protokolle TCP und IP statt. Daten werden mit einem eindeutigen Schlüsselwert versehen (vergleichbar mit dem Namen einer Variablen) und als Zeichenketten im Arbeitsspeicher abgelegt. Um das Abspeichern von Datentypen wie Ganz- oder Fließkommazahlen sowie Objekten zu ermöglichen, werden diese Daten durch die meisten Programmbibliotheken im Vorfeld serialisiert.

Mit Memcached ist es möglich, Daten dauerhaft oder nur temporär zu speichern, ein Mechanismus zum automatischen Löschen temporärer Daten ist in Memcached eingebaut.




***

Nächstes Kapitel: [5. Dokumentendatenbanken][kap5]  

[kap5]:             ./5_dokumenten_db.md "Dokumentendatenbanken"

[img-redis]:      ./img/redis.png "REDIS"
[img-mem]:      ./img/mem.png "MEM"

