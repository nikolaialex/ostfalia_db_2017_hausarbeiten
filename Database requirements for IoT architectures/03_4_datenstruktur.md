# Datenstruktur

Für die Auswahl des richtigen Datenbanksystems und den Anforderungen an dieses, ist es wichtig die zu erwartenden Datenstruktur zu betrachtet.

Ein Device stellt immer einen bzw. mehrere Werte, passend zu seiner Aufgabe zur Verfügung. Bei einem Temperatursensor mit eingebauten Hygrometer wären dies zum Einen die Temperatur und zum Anderen die relative Luftfeuchtigkeit. Zusammen mit dem Kontext ergeben sich somit für dieses Beispiel die folgenden Daten:

* ID des Sensors(Kontext)
* Messzeitpunkt (Kontext)
* Standort (Kontext)
* Temperatur
* Einheit Temperatur (Kontext)
* relative Luftfeuchte
* Einheit relative Luftfeuchte (Kontext)

Die über den Kontext bezogenen Daten werden durch ein IoT-Gerät oder einem Gateway hinzugefügt, da die Sensoren diese selbst nicht ausgeben, sondern über Spezifikationen implizit vorgeben (Einheit) oder durch Abfrage anderer Sensoren (Standort) ermittelt oder fest eingestellt werden.

Werden verschiedenste Arten von Devices betrachtet, zeigt sich, dass die Messdaten variieren und nur die Daten für ID und Messzeitpunkt für alle Devices immer gleich sind.

Aus Datenbanksicht handelt es sich somit um Zeitserien und schemalose Nutzdaten, die es zu speichern gilt.

Die Häufigkeit mit der diese Datenstrukturen erzeugt werden, ist jeweils durch das einzelnen Device bzw. IoT-Gerät festgelegt und kann somit nicht vorhergesagt werden.
