# TF-IDF
## Inverse-Dokumenten-Frequenz-Gewichtung und TF-IDF

Eine zur TF-Gewichtung zusätzliche Nutzung der inversen Termfrequenz idf nutzt das logarithmische Maß
um das Problem der klassenspeziﬁschen Übergewichtung zu lösen und skaliert häuﬁg
vorkommende Wörter herunter, während weniger häuﬁg vorkommende Wörter hoch
skaliert werden, da diese in der Regel eine höhere Bedeutung besitzen[^1].

![alt text][idf]


In der obigen Formel zur inversen Termfrequenz stellt *D* den Dokumentenraum
{d<sub>1</sub> , d<sub>2</sub> , . . . , d n } mit n Dokumenten dar und |{*d* : *t* ∈ *d*}| repräsentiert die Anzahl
der Dokumente, in denen der Term *t* vorkommt.

Die Multiplikation von Termfrequenz und inverser Termfrequenz ergibt die Tf-idf-Gewichtung[^2] ω, welche Termen das größte Gewicht zuordnet, welche häuﬁg in
individuellen Dokumenten vorkommen aber selten im gesamten Dokumentenraum.

![alt text][tfidf]

Um das Gewicht der nun noch bestehenden Ausreißer zu minimieren, kann aus den
inversen Termfrequenzen eine Diagonalmatrix gebildet und mit der Termfrequenz
multipliziert werden, um diese anschließend reihenweise mittels euklidischer Norm zu regularisieren,
sodass die Werte jeder Reihe den Einheitswert von 1 als Summe der
quadrierten Tf-idf-Gewichtungen der Dokumentvektoren ergeben.

[idf]: images/idf.gif "inverse document frequency"
[tfidf]: images/tfidf.gif "term frequency inverse document frequency"

[^1]: A vector space model for automatic indexing, publiziert im Magazin ‘Communication of the ACM’, Vol.18. S. 613–620

[^2]: Engl. Term Frequency - Inverse Document Frequency- weighting.

