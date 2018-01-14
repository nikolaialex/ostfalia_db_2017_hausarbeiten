# Nearest Neighbor Search

Die Nearest Neighbor Search (NNS) ist eine Methode aus dem Bereich des überwachten maschinellen Lernens (siehe auch 
[Exkurs Data Mining und maschinelles Lernen](06_exkurs_data_mining.md)). Bei der NNS sind zwei Dokumente umso ähnlicher,
je geringer ihr Abstand im Vektorraum ist[^1]. Dokumente im Vektorraum werden der Klasse des 
nahesten Dokuments zugeordnet[^2]. Der Abstand zwischen zwei Dokumenten wird durch ein geeignetes Unähnlichkeitsmaß,
wie z. B. dem Mahalanobis-Abstand oder der im folgendem Beispiel genutzten Metrik der euklidischen Norm bestimmt. Im konkreten 
Fall einer Suche in einer Datenbasis bedeutet dies, dass eine Query in den Dokumentenvektor überführt wird, um ausgehend 
von diesem, nach der Entfernung zu vorhandenen Dokumenten sortiert wird. Vorteil ist hier auch, das bei einem gefundenen
Dokument, weitere ähnliche Dokumente einfach ermittelt werden können.

![alt text][nns]

*d*(*x*,*y*) gibt hierbei den Abstand zwischen zwei Dokumenten *x* und *y* im n-dimensionalen reellen Vektorraum an.
Gibt es mehrere Minima zwischen Dokumenten, kann eine Zufallsauswahl getroffen werden, oder der 
k-Nächste-Nachbar-Klassifikator genutzt werden, welcher auch die *k* nächsten Nachbarn betrachtet. Mit dem k-Nächste 
 Nachbar-Klassifikator werden so Überanpassungen[^3] durch statistische Ausreißer minimiert. Um die Suche dem 
jeweiligen Anwendungsfall anzupassen, bietet es sich an, die Features durch Gewichte *w*<sub>i</sub> zu skalieren:

![alt text][weighted_nns]



[nns]: images/nns.gif "Abstandsberechnung"

[weighted_nns]: images/weighted_nns.gif "Gewichtete Abstandsberechnung"

[^1]: Künstliche Intelligenz: Eine praxisorientierte Einführung, ISBN: 978-3-658-13549-2

[^2]: Data Mining: Modelle und Algorithmen intelligenter Datenanalyse, ISBN: 978-3-8348-2171-3

[^3]: Engl. Overfitting 

