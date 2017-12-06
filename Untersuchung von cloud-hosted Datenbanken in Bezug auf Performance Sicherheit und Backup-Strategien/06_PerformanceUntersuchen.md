
# 3. Performance Untersuchen
https://www.youtube.com/watch?time_continue=88&v=wBCDe5964-0
Behandelt Skalierbarkeit, Performance und verweist am Ende auf viel Dokumentation

Infos aus dem Video:
* -> Item Cache (GetItem, PutItem) = key value Prinzip
* -> Query Cache (query, scan) = query text, get result
* -> Scale Up (Memory erhöhen) vs Scale out (Replikationen)

Cache Eviction = LRU and TTL

Anwendungsfälle:
> Wenn Sie große Datenmengen speichern möchten, auf die nur selten zugegriffen wird, ist DynamoDB eventuell nicht die richtige Lösung für Sie. Wir empfehlen Ihnen in diesem Fall die Nutzung von S3.

Beispielsweise beginnt ein Online-Spiel nur mit wenigen tausend Benutzern und mit einer geringen Datenbankarbeitslast von nur 10 Schreib- und 50 Lesevorgängen pro Sekunde. Wenn das Spiel jedoch erfolgreicher wird, wächst seine Benutzeranzahl möglicherweise schnell auf mehrere Millionen an und erzeugt zehntausende (oder sogar hunderttausende) von Schreib- und Lesevorgängen pro Sekunde. Auch können pro Tag mehrere Terabyte an Daten erzeugt werden. Wenn Sie Ihre Anwendungen für Amazon DynamoDB entwickeln, können Sie klein anfangen und Ihre Anforderungskapazität für eine Tabelle erweitern, sobald Ihre Bedarf steigt und zwar, ohne dass es dabei zu Ausfallzeiten kommt.

Amazon DynamoDB befasst sich mit den Kernproblemen von Datenbankskalierbarkeit, -verwaltung, -leistung und -zuverlässigkeit, verfügt jedoch nicht über den gesamten Funktionsumfang einer relationalen Datenbank. Komplexe relationale Abfragen (z. B. Joins) oder komplexe Transaktionen werden nicht unterstützt. Wenn Sie für Ihre Anwendung diese Funktionalität benötigen oder wenn Kompatibilität mit einer vorhandenen relationalen Engine erforderlich ist, ist es eventuell besser, wenn Sie eine relationale Engine mit Amazon RDS oder Amazon EC2 ausführen.
    1. Was für Fälle gibt es, wo Performance wichtig ist (Item Cache, Query Cache, Scale Up, Scale out?, Cache Methoden)
    2. Performance bei letzten 24h Daten
    3. Performance bei Daten >24h - 4 Monate
    4. Perforamnce bei Daten >4 Monate - Unendlich?
    5. Performance bei Fehlerfällen (Server sind down, Hurricane zerstört Datenzentrum)
    6. Performance Updates und Deletes vs. Read und Write
    7. Performance bei Peeks (z.B. Weihnachtsgeschäft)
    8. Perforamnce Queries: Sind sie unterschiedlich komplex
#### Was für Fälle gibt es, wo Performance wichtig ist (Item Cache, Query Cache, Scale Up, Scale out?, Cache Methoden)
#### Performance bei letzten 24h Daten
#### Performance bei Daten >24h - 4 Monate
#### Perforamnce bei Daten >4 Monate - Unendlich?
#### Performance bei Fehlerfällen (Server sind down, Hurricane zerstört Datenzentrum)
#### Performance Updates und Deletes vs. Read und Write
#### Performance bei Peeks (z.B. Weihnachtsgeschäft)
#### Perforamnce Queries: Sind sie unterschiedlich komplex


------------

[vorheriges Kapitel](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/2_GrundlagenUndSzenario.md)
   |   [nächstes Kapitel](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/4_SicherheitUndBackupStrategien.md)