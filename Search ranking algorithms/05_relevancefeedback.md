# Relevance Feedback

Unter dem Relevanzfeedback versteht man eine Rückmeldung vom Benutzer, zur Qualität des gelieferten Ergebnisses[^1]. 
Hierbei gewichtet der Nutzer die Resultate einer Query nach Relevanz. Basierend auf diesem Feedback wird eine
verbesserte Repräsentation vom System berechnet. Möglich sind hier mehrere Iterationen bis zum optimalen Ergebnis.

## Pseudo Relevance Feedback

Automatisiertes Feedback, anstatt Feedback durch den Nutzer. 
1. Erstelle gerankte Liste mit Resultaten der Suche.
2. Nehme an das die ersten *k* Dokumente relevant sind.
3. Führe Algorithmus zum Relevance Feedback durch.

Diese Variante funktioniert in der Regel gut, kann jedoch zur Verstärkung von Ausreißern führen. Eine iterative 
Anwendung des Algorithmus kann zu einem Drift führen.
    

[^1]:http://www.cis.uni-muenchen.de/kurse/kristof/ir/vorlesung/sitzung8.pdf