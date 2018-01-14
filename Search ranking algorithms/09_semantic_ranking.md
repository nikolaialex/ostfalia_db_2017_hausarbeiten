# Semantisches Ranking

Um Suchergebnisse an die Erwartungen der Nutzer anzupassen, soll in diesem Abschnitt die semantische Suche erläutert 
werden. Der Autor dieser Arbeit bezieht sich hierbei auf das Kompendium semantische Netze von Klaus Reichenberger[^1] 
und dem Titel Semantische Technologien von Andreas Dengel[^5].

Im Gegensatz zur einfachen Schlüsselwortsuche, bei der die Ergebnisse durch eine Übereinstimmung der Suchwörter mit
den Wörtern in der Textmenge generiert werden, soll eine sprachliche Realität mit Synonymen, Homonymen, Abstraktion,
beispielhafter Sprache und Umschreibungen abgebildet werden. Dies geschieht, indem versucht wird, mit
kognitiven Algorithmen die Suchanfrage sowie die zu durchsuchenden Texte zu verstehen.
 
In spezifischen Anwendungsfällen ist es oftmals wichtig, den Domänenkontext, also die Arbeit des Nutzers zu kennen.
Wichtig kann beispielsweise auch die Information sein, ob ein Dokument, eine Dokumentenart, einer Telefonnummer
oder nach einem Datum der Veröffentlichung gesucht wird. Im Folgendem wird ein Vorgehen erläutert, in dem 
Suchtechnologien, linguistische Techniken und semantische Netze kombiniert werden, um der Mehrdeutigkeit natürlicher Sprache
gerecht zu werden.

## Der Suchprozess

Hier wird versucht, mit einem semantischen Netz, die Lücke zwischen der Suchmaschine, welche reine Zeichenketten erkennt
und dem Nutzer, welcher über Themen und Zusammenhänge operiert, zu schließen. Diese beiden Ebenen der Interpretation gilt es 
mit einem semantischen Netz als Vermittler, zu verknüpfen.

![alt text][interpretationsebenen]

In der Abbildung oben stellt die Themenebene den Bereich dar, welcher den Inhalt der Dokumente beschreibt (Makrotechnik).
Mit der Objektebene ist der Bereich gemeint, welcher erkennen lässt, ob das konkrete Suchwort auch im Text enthalten ist 
(Mikrotechnik). Das Problem der Mehrdeutigkeit bei der Übersetzung zwischen den einzelnen Ebenen soll minimiert werden, indem 
Probleme in Teilaufgaben eingeteilt werden.

Suchanfragen der Benutzer sind oftmals mehrdeutig und unvollständig, wird jedoch der gedachte Kontext hinzugefügt, ist 
es möglich, die Nutzeranfrage korrekt zu interpretieren.

Mithilfe der klärenden Interaktion ist es möglich, dass der Nutzer mit Suchbegriffen startet und vom System eine Antwort
erhält, welche die Ergebnismenge in Gruppen ähnlicher Dokumente unterteilt, aus der ein Nutzer wiederum die benötigten
Dokumente auswählen kann.

## Makrotechniken

Aufgabe der Makrotechnik ist es, den Inhalt der Dokumente zu analysieren und Themen zuzuordnen. Gelieferte Dokumente
sollen dem entsprechen, was dem Nutzer interessiert. Hierfür wird zum einen die semantisch angereicherte Volltextsuche 
betrachtet, welche gut für spezifische, kurze Dokumente skaliert. Zum anderen wird die automatische Klassifikation 
erläutert, welche vorzugsweise bei längeren Texten mit allgemeinsprachlichen Vokabular einsetzbar ist.

Bei der semantisch angereicherten Volltextsuche werden die vom Benutzer gewählten Wörter automatisch mit inhaltlich ähnlichen
Text angereichert. Dieser ähnliche Text wird aus einem definierten semantischen Netz formuliert.
Die Qualität der Ergebnisse wird mit dem Recall und der Precision gemessen 
(siehe [Exkurs Data Mining und maschinelles Lernen](06_exkurs_data_mining.md)). Semantische Erweiterungen, wie z. B. die
Anreicherung mit Synonymen, erhöhen i. d. R. den Recall (mehr relevante Dokumente werden gefunden), verschlechtern aber 
oftmals auch die Precision, da auch mehr irrelevante Dokumente durch Wortverwandtheit gefunden werden. Diesen Effekt 
gilt es durch eine entsprechende Sortierung nach Relevanz (beispielsweise [TF-IDF](07_tfidf.md)) für den Nutzer aufzuheben. 

Werden in einem Dokument häufig sprachliche Mittel wie Synonym, Umschreibung, Abstraktion und Konkretisierung genutzt,
kann es vorkommen, dass relevante (Such-) Begriffe selber nicht im Text vorkommen. Um dieser Problematik beizukommen,
kann man sich statistischer Textmining-Verfahren bedienen. Methoden des maschinellen Lernens, speziell der automatischen 
Klassifikation können für eine thematische Einordnung solcher Dokumente sorgen. Anstatt einzelner Wörter wird hierbei
die Ähnlichkeit der Dokumente gemessen. Vorteil hierbei ist, dass der manuelle Aufwand auf die initiale Einrichtung 
beschränkt ist. Für das Training müssen den vorkommenden Klassen i. d. R. mindestens fünf Beispieldokumente zugeordnet werden.
Nachteil ist, dass statistische Verfahren oftmals eine große Unschärfe in Bezug auf die Nutzererwartung haben. Diese
können jedoch wiederum mit einer Ergänzung von regelbasierten Algorithmen minimiert werden. 

## Mikrotechniken

Um Begriffe zuverlässig in Dokumenten zu finden, ist es notwendig, auch unterschiedliche Formen der Suchbegriffe, wie 
Flektionsformen[^2], unterschiedliche Schreibweisen, orthografische Fehler [^3], Tippfehler, Formulierungsvarianten,
Umschreibungen und unterschiedliche Synonyme zu verarbeiten. Nominalgruppen und Namen, die aus mehreren Worten bestehen, 
kommen hierbei eine besondere Bedeutung zu. Um einen Zeichenkettenvergleich durchzuführen, welche auch diese 
unterschiedlichen Formen der Suchbegriffe beachtet, sind Techniken wie das Stemming, Ähnlichkeitsmaße wie die 
Trigramm-Ähnlichkeit oder die Edit-Distance, Phonetische Abbildung, Entity Recognition sowie Namensvarianten und 
Synonyme geeignet. 



[interpretationsebenen]: images/interpretationsebenen.png "Verschiedene Interpretationsebenen zwischen Zeichenketten im Text 
und der Aufgabe des Nutzers. Entnommen aus [^1], Abb. 10.1."
 
 [^1]: Kompendium semantische Netze, ISBN: 978-3-642-04315-4
 
 [^5]: Semantische Technologien: Grundlagen - Konzepte, Anwendungen, ISBN: 978-3-8274-2663-5
 
 [^2]: Beispiel: Vater wird zu Väter, Kaffeemaschine wird zu Kaffemaschinen
 
 [^3]: I. d. R. Rechtschreibfehler