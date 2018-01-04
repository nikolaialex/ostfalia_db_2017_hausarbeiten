# PageRank

Der PageRank Algorithmus ist ein Meilenstein der Suchmaschiee von Google[^1]. Dabei liefert das Random Surfer Model[^2]
die Berechnungsgrundlage. Der Algorithmus analysiert die Verknüpfungen zwischen Dokumenten und lässt sich
dementsprechend gut auf Webseiten oder aber auch wissenschaftliche Arbeiten mit einer hohen Anzahl von Zitationen 
nutzen. Da der PageRank Algorithmus von Google für das Internet erfunden wurde, bezieht sich die folgende Erläuterung
auf Webseiten und dem Random Surfer als Nutzer.

## Random Surfer Model

Das Modell bildet das Verhalten von Internetnutzern ab, indem die Wahrscheinlichkeit berechnet wird, mit der ein 
beliebiger Nutzer eine Webseite besucht. Ein Internetnutzer, der Surfer, navigiert durch die Eingabe definierter 
Adressen oder über Verlinkungen auf besuchten Webseiten durch das Internet. Der Algorithmus bedient sich der Annahme, 
dass die Auswahl der angeklickten Links zufällig geschieht und damit unabhängig vom Inhalt ist. Weiterhin wird
angenommen, dass nach einer bestimmten Anzahl von gefolgten Links wieder eine definierte Adresse eingegeben wird.

## PageRank Wahrscheinlichkeit

PageRank (PR) beschreibt die Wahrscheinlichkeit, mit der eine Webseite von einem beliebigen Surfer besucht wird. Es 
verwendet die Struktur der Links zwischen Webseiten. Es werden in-links sowie out-links analysiert. Umso mehr
Verlinkungen auf eine Webseite zeigen, desto höher wird die Webseite gewichtet. Da PR ein rekursiver Algorithmus ist, 
wird der PR durch die in-links, sowie der Gewichtung der Webseiten die darauf verlinken, bestimmt. Der PageRank einer
Webseite wird somit gleichmäßig über alle out-links auf die verlinkten Seiten übertragen.

Die Wahrscheinlichkeit, dass der Random Surfer nach einem Schritt eine bestimmte Webseite *X* besucht, kann durch die
Relation
 
 *p*(*X*)<sub>1</sub> = (*Anzahl der out-links)*<sup>-1</sup>
  
beschrieben werden. *p*(*X*) = 0 bedeutet, dass kein Link von der vorhergehenden Seite auf die Seite *X* führt[^3].
Da es sich beim PageRank um eine Wahrscheinlichkeit handelt, ergibt die Summe der PageRanks aller Webseiten immer eins.
Das Folgen eines Links wird hierbei als Iteration verstanden.
Bei der zweiten Iteration werden die Relationen, gebildet aus der Multiplikation von *p*(*X*)<sub>1</sub> mit der Anzahl
der in-links möglicher Vorgänger, addiert: 

*p*(*X*)<sub>2</sub> = *p*(*X*)<sub>1</sub> \* *Anzahl der in-links von Vorgänger A* + *p*(*X*)<sub>1</sub> \*
 *Anzahl der in-links von Vorgänger B* + ...
 
Eine geringere Anzahl an out-links hat somit zur Folge, dass der PageRank zu einem höheren Anteil auf eben diese
Links übertragen wird. 

Die folgende Formel stellt den PageRank für alle Webseiten dar:

![alt text][pagerank]

*T*<sub>*i*</sub> ist eine der *n* Seiten, welche 
auf Seite *A* zeigt. *C*(*T*<sub>*i*</sub>) definiert die Anzahl der out-links von Seite *A*. Um eine 
Wahrscheinlichkeitsverteilung über alle Webseiten zu erhalten, fließt mit *N* die Gesamtanzahl der betrachteten Webseiten 
in die Formel mit ein. *d* beschreibt einen Dämpfungsfaktor, welcher im Standard auf 0.85 gesetzt ist. Er bezeichnet
die Wahrscheinlichkeit, mit der ein Nutzer zu einer anderen zufälligen Seite wechselt.


[pagerank]: images/pagerank.gif "PageRank von Sergey Brin and Lawrence Page"

[^1]: https://bvicam.ac.in/news/INDIACom%202008%20Proceedings/pdfs/papers/281.pdf

[^2]: https://de.ryte.com/wiki/Random_Surfer_Model

[^3]: Mathematik und Technologie, ISBN: 978-3-642-30092-9

[^4]: http://infolab.stanford.edu/pub/papers/google.pdf