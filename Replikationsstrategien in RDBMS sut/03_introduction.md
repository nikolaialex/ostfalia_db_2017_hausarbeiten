# Einleitung

Datenbanken stehen heutzutage im Kern vieler Anwengungen und Systeme. 

http://www.itwissen.info/Replikation-replication.html

Es gibt verschiedene Replikationsverfahren, die in den folgenden Kapiteln näher betrachtet werden.



Hauptziel beim Einsatz replizierter Datenbanken ist die Steigerung der Verfügbarkeit
 eines Verteilten DBS. Denn repliziert an mehreren Knoten gespeicherte 
Daten bleiben auch nach Ausfall eines der Rechner erreichbar. Daneben 
wird auch eine Verbesserung der Leistungsfähigkeit angestrebt, 
insbesondere für Lesezugriffe. So lassen sich viele 
Kommunikationsvorgänge einsparen, indem man Objekte repliziert an allen 
Knoten speichert, an denen sie häufig referenziert werden (Unterstützung
 von Lokalität). Zum anderen erhöht die Wahlmöglichkeit unter mehreren Kopien das Potential zur Lastbalancierung,
 so daß die Überlastung einzelner Rechner eher vermeidbar ist. Unter 
diesen Gesichtspunkten bieten natürlich voll-redundante Datenbanken, bei
 denen jeder Knoten eine Kopie der gesamten Datenbank führt, die größten
 Vorteile; insbesondere können prinzipiell alle Lesezugriffe lokal 
abgewickelt werden. 

vollständige, teilweise Replikation

###Replikationsstrategien

* Korrektheitskriterium / Synchronisation 

* 1-Kopien-Äquivalenz: 

  * vollständige Replikationstransparenz: jeder Zugriff liefert jüngsten transaktionskonsistenten Objektzustand 
  * alle Replikate sind wechselseitig konsistent
  * Serialisierung von Änderungen

* geringere Konsistenzanforderungen 

  * Zugriff auf ältere Objektversionen
  * evtl. keine strikte Serialisierung von Änderungen, sondern parallele Änderungen mit nachträglicher Konflikt-Behebung

* symmetrische vs. asymmetrische Konfigurationen 

  * symmetrisch: jedes Replikat gleichartig bezüglich Lese- und Schreibzugriffen; n Knoten können Objekt ändern 

*  asymmetrisch(Master/Slave, Publish/Subsribe-Ansätze) 

  * Änderungen eines Objekts an 1 Knoten (Master, Publisher, Primärkopie)
  * Lesen an n Knoten (Slave, Subscriber, Sekundärkopien) 
  * Kombinationen (z.B. Multi-Master-Ansätze)

* Zeitpunkt der Aktualisierung: synchron oder asynchron

  * synchron (eager): alle Kopien werden durch Änderungstransaktion 

  aktualisiert 

  * asynchron (lazy): verzögertes Aktualisieren (Refresh)