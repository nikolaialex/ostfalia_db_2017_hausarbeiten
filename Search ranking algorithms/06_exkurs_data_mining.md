# Exkurs
## Data Mining und maschinelles Lernen

Data Mining ist eine Technik aus dem Bereich der Data Science und beschäftigt
sich mit der Extrahierung des Sinnes aus Daten. Die Generierung von Wissen durch
maschinelles Lernen steht im Vordergrund aber auch die Vorbereitung der Daten
mit semantischen Mitteln und die Aufbereitung zur Nutzung und Visualisierung[^1].

Im Gegensatz zum Data Mining beschreibt maschinelles Lernen Verfahren, um mit
Maschinen mittels Algorithmen in Daten ein Muster zu erkennen und verborgene
Informationen zu extrahieren. Aber auch um Vorhersagen treﬀen zu können und Algorithmen
dahin gehend zu automatisieren, dass sie mit den gewonnenen Informationen
diesen Ablauf optimieren, um beim nächsten Durchlauf eine höhere Genauigkeit zu
erzielen[^2].

Das Anwenden von Methoden des maschinellen Lernens auf große Datenmengen verschiedenster 
Anwendungen wird als Data Mining bezeichnet. Hierbei sollen einfache
Modelle mit einem hohen Nutzen generiert werden und spätere Rückschlüsse nutzbar
gemacht werden, um den jeweiligen Prozess weiter zu verbessern.

![alt text][dm_overview]

In der obigen Abbildung ist die Einbettung von Data Mining im maschinellen Lernen
dargestellt. Die für das Data Mining genutzten Methoden basieren auf denen des
induktiven maschinellen Lernens, d. h. aus großen Datenmengen mit geringem Wissen
darüber, neue Zusammenhänge aufzudecken. Im Gegensatz hierzu verfolgt deduktives maschinelles 
Lernen das Ziel, gegebenes Wissen kleinerer Datenmengen zu
optimieren.

Ein Teilbereich des induktiven Lernens ist das überwachte Lernen[^3], hierzu gehört
die Klassiﬁkation. Ein weiterer Teilbereich des induktiven Lernens ist das unüberwachte Lernen[^4], welcher sich in 
Assoziationen und Segmentierung aufteilt.

## Methoden des Data Mining
### Überwachtes Lernen

Hauptaussage des überwachten Lernens ist die der Klassiﬁkation; eine Aufteilung
einer gegebenen Menge nach vorgegebenen Klassen. Es wird zwischen den der Tabelle
unten angegebenen Klassiﬁkatoren unterschieden[^2].

![alt text][classifier]

Der Klassiﬁkator wird durch einen iterativen Vorgang des Trainings und der Auswertung 
generiert und verbessert. Als Basis hierfür dient immer ein Datensatz mit bereits
klassiﬁzierten Daten, welcher oftmals in einen Trainings- und Testsatz aufgeteilt
wird. In jedem Lernschritt – also jeder Iteration – wird der Fehler der Klassiﬁkation
ausgewertet und abhängig vom jeweiligen Modell, Maßnahmen zur Verbesserung
angewendet.

Funktioniert der Klassiﬁkator mit dem Trainingssatz gut aber mit dem Testsatz
weniger, so spricht man von einer Überanpassung[^5]. Wenn der Testsatz besser validiert
wird, spricht man von einer guten Generalisierungsfähigkeit. Generalisierungsfähigkeit
und Überanpassung stehen im Widerspruch zueinander und es ist immer ein auf
den Anwendungsbereich angepasster Mittelweg zu wählen.

### Unüberwachtes Lernen

Im Gegensatz zum überwachten Lernen steht das unüberwachte Lernen für die
Analyse von Datenbeständen mit geringem Wissen darüber, also die Analyse von
unklassiﬁzierten Daten. Somit kann zwar die Anzahl der Gruppen, jedoch nicht die
Gruppenzugehörigkeit direkt beeinﬂusst werden.

Im Wesentlichen lassen sich die Methoden des unüberwachten Lernens in die beiden
Klassen Assoziation und Segmentierung aufteilen. Assoziierende Methoden eignen
sich um den Zusammenhang der Daten zu analysieren, während die der Segmentierung
für eine Aufteilung der Daten in Teilabschnitte sorgt. Segmentierende Methoden
sind i. d. R. Clusteranalyseverfahren und benötigen einen Datenbestand mit gleichem 
Informationsumfang, während assoziierende Methoden nur einen grundsätzlich
vergleichbaren Datenbestand voraussetzen.

## Abbildung auf Vektoren
Um eine Gewichtung von Suchergebnissen auf textuelle Daten anwenden zu können, ist eine Tokenisierung und Vektorisierung 
notwendig. Unter einer Tokenisierung versteht man die Segmentierung von Dokumenten in
linguistische Einheiten[^8,11], wobei hier der Einfachheit
halber mit einer Aufteilung in Wörter gearbeitet wird. Die Überführung in ein Vektorraummodell[^9] ist eine Methode zum
Finden von Informationen, bei dem die entsprechenden Informationen auf einen mehrdimensionalen Vektorraum abgebildet
werden[^10].

Ein Dokument, bestehend aus seinen Wörtern, wird im Vektorraum durch seine Terme
repräsentiert. Als Terme werden die charakteristischen Wörter des Modells bezeichnet,
welche nach ihrer Wichtigkeit gewichtet werden. Jeder Vektorraum hat *t* Dimensionen,
basierend auf *t* unterschiedlichen Termen. Die Termfrequenz *tf* (*t*, *d*) gibt an, wie oft
der Term *t* im Dokument *d* vorkommt. Der Dokumentenvektor besteht wiederum aus
den Termfrequenzen ![alt text][document_vector] aus dem die
Feature Matrix, bestehend aus allen Dokumentenvektoren, gebildet wird. In Bezug
auf den Vektorraum werden die Terme auch als Features bezeichnet.

Zur Verdeutlichung des Vektorraums folgt ein Beispiel mit zwei Dokumenten d<sub>0</sub> und
d<sub>1</sub>.

d<sub>0</sub> = Krankenpﬂeger zum nächstmöglichen Termin gesucht.

d<sub>1</sub> = Ingenieur mit Erfahrung gesucht, suchen Ingenieur für eine Vollzeitstelle.
 
Durch eine Tokenisierung ergibt sich folgende Zuordnung:

![alt text][vector_table]

Wie in der obigen Tabelle zu sehen, werden die Wörter kleingeschrieben und alphabetisch
sortiert.

Somit ergeben sich folgende Vektoren, welche zeilenweise in einer Feature Matrix
gespeichert werden:
 
![alt text][feature_matrix]

Da eine visuelle Darstellung im mehrdimensionalen Raum mit zunehmender Dimensionalität
unübersichtlicher wird, zeigt die Abbildung unten eine vereinfachte Repräsentation
mit drei Termen und einem dritten Dokument d<sub>2</sub> , wobei die Winkel α und β das
Distanzmaß von ![alt text][vd0] zu ![alt text][vd1] und ![alt text][vd2] darstellen.


![alt text][vector_dia]

## Stammformenreduktion
Da Wörter mit der gleichen Bedeutung in Texten oftmals in vielfältiger Form vorkommen, gibt es verschiedene
Lösungsansätze zur Reduzierung der Datenmenge. 
Eine davon ist die Normalisierung von Wörtern auf ihren Wortstamm, auch als Stammformenreduktion[^13]
bezeichnet. Ein bekanntes Verfahren hierzu ist das Stemming, welches eine lexikonfreie Suffixanalyse
vornimmt und somit für viele Sprachen gültig ist.

![alt text][stemming]

In der obigen Tabelle sind an einem Beispiel zwei Ausprägungen der Stammformreduktion verdeutlicht. Die Stammreduktion verschiedener Wörter auf die gleiche Normalform wird als Overstemming bezeichnet. Beim Understemming werden wiederum mehrere Normalformen für das gleiche Wort gebildet.

Da Stemming eine Komprimierung von Wörtern ist, ist hiermit auch immer ein Informationsverlust verbunden 
und es muss ein Kompromiss zwischen dem Overstemming und dem Understemming gefunden werden. 

## Gütekriterien

Um eine objektive Beurteilung der Ergebnisse einer Suche mit bewährten Kriterien durchzuführen, werden im Folgenden 
der positive Vorhersagewert und die Richtig-Positiv-Rate vorgestellt, welche die Qualität eines Suchergebnisses angeben.

### Positiver Vorhersagewert
Der als positiver Vorhersagewert[^6] bezeichnete Anteil richtig positiv erkannter Dokumente, 
in Bezug auf alle erkannten positiven Dokumente, auch als Genauigkeit bezeichnet, 
wird durch folgende Formel ausgedrückt.

![alt text][precision]


### Richtig-Positiv-Rate
Die Richtig-Positiv-Rate[^7], auch als Sensitivität bekannt, gibt den Anteil der richtig positiv
erkannten Dokumente zu den tatsächlich positiven Dokumenten an:

![alt text][recall]


[dm_overview]: images/dm_overview.png "Einbettung von Data Mining im maschinellen Lernen. In Anlehnung an [^2], S.61"
[precision]: images/precision.gif "Einbettung von Data Mining im maschinellen Lernen. In Anlehnung an [^2], S.61"
[recall]: images/recall.gif "Einbettung von Data Mining im maschinellen Lernen. In Anlehnung an [^2], S.61"
[classifier]: images/classifier.png "Klassifikatoren des überwachten Lernens"
[document_vector]: images/document_vector.gif "Dokumentenvektor"
[vector_table]: images/vector_table.png "Tokenisierte Wörter"
[feature_matrix]: images/feature_matrix.png "Feature Matrix"
[vd0]: images/vd0.gif "Vektor 0"
[vd1]: images/vd1.gif "Vektor 1"
[vd2]: images/vd2.gif "Vektor 2"
[vector_dia]: images/vector_dia.png "Vektor Representation"
[stemming]: images/stemming.png "Beispiele zu Over- und Understemming"

[^1]: DATA MINING: Practical Machine Learning Tools and Techniques, ISBN: 0-12-088407-0

[^2]: Data Mining: Einsatz in der Praxis (Allgemein: Datenbanken), ISBN-13: 978-3827313492

[^10]: A vector space model for automatic indexing, publiziert im Magazin 'Communication of the ACM', Vol.18. S. 613–620

[^11]: Computerlinguistik und Sprachtechnologie, ISBN: 978-3-8274-2224-8

[^13]: Morphologische Relationen durch Reduktionsalgorithmen, publiziert in 'Nachrichten für Dokumentation'. S. 168-172.  
ISSN: 0027-7436


[^3]: Engl. supervised learning

[^4]: Engl. unsupervised learning

[^5]: Engl. Overﬁtting

[^6]: Engl. Precision

[^7]: Engl. Recall

[^8]: Z. B. Wörter, Phrasen, Sätze, Absätze oder Diskursabschnitte.

[^9]: Engl. Vector Space Model, kurz: VSM.

[^12]: Auch: Analyse von Nachsilben ohne den Vergleich mit einem Wörterbuch.

