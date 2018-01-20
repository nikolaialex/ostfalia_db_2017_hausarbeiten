Ergebnisse
==========

Im folgenden Abschnitt werden die Ergebnisse der Abfragenvergleiche
gegenübergestellt. Da die SQL- und Cypher Abfragen unter Umständen Längen von
einer Seite Text oder mehr aufweisen können, sind in den Ergebnissen lediglich
Auszüge davon zu lesen. Die kompletten Abfragen befinden sich im Anhang.

Imports von Daten aus einer CSV-Datei
-------------------------------------

Einerseits wurde hierbei überprüft wie schnell der Import einer CSV-Datei in der
relationalen Datenbank SQL Server und der Graph Datenbank Neo4J. Andererseits
wurde überprüft, ob die Menge der zu importierenden Daten eine Auswirkung auf
den Import hat. Dazu wurde der Datensatz für jeden Fall beschnitten und in 10
Ausfertigungen verwendet. Jeder Teildatensatz bestand aus den ersten 200 bis
102400 Zeilen *(100*2<sup>n</sup> | n = 1, ..., 11)* des Originaldatensatzes.

### Unterschiede der Abfrage

SQL Server und Neo4J realisieren den Import auf unterschiedliche Art und Weise.
Die SQL Abfrage ist zweigeteilt, sie beinhaltet einerseits die Erstellung der
Tabelle und die Deklaration der Spaltentypen und andererseits den Import der
CSV-Datei in diese Tabelle. Die Cypher Abfrage benötigt NoSQL-typisch keine
Tabellenerstellung. Allerdings müssen die „Spalten“ der CSV-Datei den
Eigenschaften der anzulegenden Entität mit Datentyp zugewiesen werden. Die
Komplexität der beiden Abfragen ist daher sehr ähnlich.

In SQL:
~~~roomsql
CREATE TABLE [dbo].MENTIONS_IMPORT(
[postid] [int] PRIMARY KEY,
[sourceparam] [nvarchar](255) NULL,
/** ... **/
);
~~~
~~~roomsql
BULK INSERT [dbo].MENTIONS_IMPORT
FROM '...\\MSE_Mentions_102400.txt'
WITH (FIELDTERMINATOR = '\\t', ROWTERMINATOR = '\\n',
FIRSTROW = 2, DATAFILETYPE = 'widechar')
~~~

In Cypher:
~~~roomsql
LOAD CSV WITH HEADERS FROM 'file:///MSE_Mentions_102400.txt' AS line FIELDTERMINATOR '\\t'
CREATE (a:POST  {PostID: toString(line.externalpostid),
                 SocialNetwork: toString(line.sourceparam),
                 // ...
})
~~~

### Geschwindigkeit unterschiedlicher Importmengen

Die Erstellung der benötigten SQL Tabelle dauert beim ersten Mal 20
Millisekunden und wird für den Vergleich zu jedem CSV Import addiert. Es wurden
jeweils drei Abfrage durchgeführt, in Tabelle 1 ist der Durchschnitt dieser
Abfragen aufgelistet. Um auf eine eventuelle Linearität zu überprüfen, wurde
berechnet wie viele Anfragen jeweils pro Nanosekunde durchgeführt werden.

Tabelle 1: Durchschnittliche Dauer der Abfragen

| n      | Dauer SQL | Anfragen pro ns | Dauer Cypher | Anfragen pro ns |
|--------|-----------|-----------------|--------------|-----------------|
| 200    | 0,030     | 151,667         | 0,133        | 666,667         |
| 400    | 0,042     | 105,833         | 0,176        | 440,000         |
| 800    | 0,052     | 65,000          | 0,223        | 278,750         |
| 1600   | 0,077     | 47,917          | 0,291        | 181,875         |
| 3200   | 0,141     | 44,063          | 0,399        | 124,688         |
| 6400   | 0,244     | 38,177          | 0,667        | 104,271         |
| 12800  | 0,447     | 34,896          | 1,209        | 94,427          |
| 25600  | 0,813     | 31,771          | 2,490        | 97,266          |
| 51200  | 1,589     | 31,042          | 4,882        | 95,352          |
| 102400 | 3,383     | 33,040          | 10,931       | 106,751         |

In beiden Abfragen scheint ein Overhead zu bestehen, der sich in den ersten
Abfragen (bis n = 3200) bemerkbar macht. Bei größeren Abfragen steigt die Dauer
linear an. Dabei ist der Import in den SQL Server mit 33 Zeilen pro Nanosekunde
ungefähr dreimal schneller als der Import von 100 Zeilen pro Nanosekunde in
Neo4J.

![](../../../Desktop/Graphenorientierte%20vs%20relationale%20Datenbank%20-%20Vergleich%20der%20Speicherung%20und%20Abfrage%20bei%20der%20Analyse%20von%20Social%20Data/media/09-performanz-von-sql-und-cypher-beim-csv-import.png)

Abbildung 9: Performance von SQL und Cypher bei CSV Import

Import des Datensatzes
----------------------

Der Import der Daten ist für jede Datenbank optimiert worden und unterscheidet
sich daher im Ablauf.

Für den Import in die SQL Datenbank wurde die CSV-Datei temporär komplett
importiert, da diese als Tabelle im SQL Server sehr einfach weiterverarbeitet
werden kann. Im Anschluss wurden die Daten aus der temporären Tabelle in die
Entitäts- und Relationstabellen kopiert. Daraufhin wurden die Primär- und
Fremdschlüssel erzeugt, um danach die Relationentabelle *Mentioned* mit den
berechneten Werten zur Entitätstabelle *Profiles* zu befüllen.

Der Import in die Neo4J Datenbank verwendet die CSV Datei mehrmals, um die
jeweils benötigten Daten zu importieren. Im ersten und zweiten Schritt werden
die *Posts* und *Profile* importiert. Daraufhin erfolgt in vier Schritten die
Erstellung der Relation *Published*. Im Anschluss werden die Daten für die
Entität *Tags* importiert und die Relation *Talks_About* berechnet und mit
Informationen befüllt. Danach werden alle möglichen Namen aus der Relation
*Published* der Entität *Profile* als Liste hinzugefügt. Es folgt die
zeitintensive Berechnung der Relation *Mentioned*.

Tabelle 2: Ablauf und Dauer des Datensatzimports

| SQL                        | Dauer      |   | Cypher                | Dauer      |
|----------------------------|------------|---|-----------------------|------------|
| Einfacher CSV Import       | 5,5 s      |   | Posts importieren     | 23,1 s     |
| In Tabellen verschieben    | 68,1 s     |   | Profile importieren   | 16 s       |
| Primär- und Fremdschlüssel | 6,3 s      |   | Publishes berechnen   | 165,2 s    |
| Mentions berechnen         | 110,4 s    |   | Tags importieren      | 79 s       |
|                            |            |   | Talks_About berechnen | 176,5 s    |
|                            |            |   | Namen kopieren        | 2 s        |
|                            |            |   | Mentions berechnen    | 26100 s    |
| **Gesamtdauer**               | **0,05 h** |   | **Gesamtdauer**       | **7,38 h** |

Der tabellarisch aufgebaute Inhalt der CSV Datei kann, wie im ersten Test schon
beobachtet, schneller in die Datenbank importiert werden. Die Werte wurden
hierbei nur einmal getestet, begründet auf der langen Kalkulation bei der
Erstellung der Relation *Mentioned* mit Cypher. Die Verarbeitung der initial im
Tabellenformat vorhandenen Informationen scheint performanter im SQL Server
verarbeitet werden zu können als in Neo4J. Der Import dauerte in dieser Form mit
Neo4J 140-mal länger. Wird die Berechnung der Mentions-Relation ignoriert, ist
die SQL Datenbank mit 1,3 Minuten um den Faktor 4 schneller als die Neo4J
Datenbank, welche 7,7 Minuten benötigt.

Einfache Abfragen
-----------------

Im folgenden Abschnitt werden grundlegende SQL Abfragen durchgeführt, um zu
überprüfen inwieweit sich die Abfragenkomplexität und die Dauer zwischen SQL
Server und Neo4J unterscheiden. Die komplette Gegenüberstellung der erhobenen
Daten befindet sich in Anhang C – Vergleichstabellen.

### Zählen (Count)

#### Anzahl der Posts insgesamt

SQL Abfrage
~~~roomsql
SELECT COUNT(*) FROM Posts
~~~

Cypher Abfrage
~~~roomsql
MATCH (a:POST) RETURN COUNT(a)
~~~

Die Komplexität der Abfrage ist sehr ähnlich. Ein Unterschied besteht in der
Geschwindigkeit der beiden Abfragen. In SQL dauert die Abfrage 21 ms, mit Cypher
lediglich 1 ms. Da die Daten in Neo4J in *fixed-size record stores* gespeichert
werden, muss lediglich die Größe der DB-Datei für Beiträge ermittelt werden.
Dies ist um den Faktor 20 schneller als das Zählen der einzelnen Einträge.

#### Anzahl der Posts, die als Bild veröffentlicht wurden

SQL Abfrage 
~~~roomsql
SELECT COUNT WHERE contenttype = 'IMAGE'
~~~

Cypher Abfrage
~~~roomsql
MATCH (a:POST) WHERE a.ContentType = 'IMAGE' RETURN COUNT(a)
~~~

In dieser Abfrage müssen i.G.z. ersten auch in Neo4J die Daten direkt ermittelt.
In SQL wird das Ergebnis nach 39 ms gezeigt, mit Cypher dauert die Abfrage
durchschnittlich 110 ms und somit fast 3x so lang. Die Komplexität beider
Abfragen ist ähnlich.

#### Anzahl der Posts, die das Wort ‚Saw‘ enthalten

SQL Abfrage
~~~roomsql
SELECT COUNT(*) FROM Posts WHERE postcontent like '%saw%'
~~~

Cypher Abfrage (a)
~~~roomsql MATCH (a:POST)  
WHERE toLower(a.PostContent) contains 'saw'  
RETURN count(a)
~~~

Cypher Abfrage (b) 
~~~roomsql
MATCH (a:POST)  
WHERE a.PostContent contains 'saw'  
OR a.PostContent contains 'Saw'  
OR a.PostContent contains 'SAW'  
RETURN count(a)
~~~

Die Abfrage, ob eine Person über Sägen geschrieben hat, erfordert einen
Stringvergleich. In SQL funktioniert dies mit dem *LIKE* Operator und
unterschiedet nicht in Groß- und Kleinschreibung. In Cypher wird stattdessen der
Operator *CONTAINS* verwendet und Groß- von Kleinschreibung unterschieden. Der
Vergleich des Strings kann somit durch Tests aller Varianten oder durch die
Funktion *TOLOWER()* auf den Beitrag. Die Komplexität und Länge (bei b) ist
dadurch leicht erhöht zur Abfrage in SQL. Die Geschwindigkeiten sind auf
ähnlichem Niveau bei knapp unter einer Sekunde. Die schnellste Abfrage wurde mit
Cypher und *TOLOWER()* durchgeführt.

### Anzeigen (SELECT TOP)

#### Die ersten 1.000 Posts anzeigen

SQL Abfrage 
~~~roomsql
SELECT TOP 1000 * FROM Posts
~~~

Cypher Abfrage 
~~~roomsql
MATCH (a:POST) RETURN a.PostContent LIMIT 1000
~~~

#### Die ersten 10.000 Posts anzeigen

SQL Abfrage 
~~~roomsql
SELECT TOP 10000 * FROM Posts
~~~

Cypher Abfrage 
~~~roomsql
MATCH (a:POST) RETURN a.PostContent LIMIT 10000
~~~

#### Die ersten 100.000 Posts anzeigen

SQL Abfrage 
~~~roomsql
SELECT TOP 100000 * FROM Posts
~~~

Cypher Abfrage 
~~~roomsql
MATCH (a:POST) RETURN a.PostContent LIMIT 100000
~~~

Die Komplexität und Länge der beiden Anfragen ist ähnlich. Bei der Durchführung
ist Neo4J 30-40% schneller, die Ermittlung der 100.000 Beiträge dauert mit SQL
2,1 Sekunden, mit Neo4J 1,5 Sekunden.

Dabei ist anzumerken, dass die Werte die Zeiten für die Berechnung ist. Die
Darstellung der Informationen erfolgt im SQL Server Management Studio fast
direkt. Die Darstellung der Informationen im Neo4J-Browser kann bei 100.000
Beiträgen mehrere Minuten in Anspruch nehmen.

Beide Anfragen verlangsamen sich mit erhöhtem Datensatz, SQL ermöglicht bei
1.000 Beiträgen eine Geschwindigkeit von ca. 100 pro Nanosekunde und verringert
sich bei 10.000 und 100.000 Beiträgen auf 27 Beiträge/ns bzw. 21 Beiträge/ns. In
Cypher ist diese Veränderung ähnlich, die Anzahl an Beiträgen die pro
Nanosekunde ermittelt werden können sind für 1.000, 10.000 und 10.000 Beiträge
57, 17 und 16.

### Aggregatfunktionen (Avg, Sum, Max)

#### Zeige den durchschnittlichen Sentiment für jeden ContentType an

SQL Abfrage 
~~~roomsql
SELECT contenttype, avg(sentimentvalue) as AvgSentiment  
FROM Posts GROUP BY contenttype ORDER BY AvgSentiment DESC
~~~

Cypher Abfrage 
~~~roomsql
MATCH (n:POST)  
RETURN n.ContentType, avg(n.Sentiment) as AverageSentiment ORDER BY
AverageSentiment DESC
~~~

Die Komplexität ist erneut sehr ähnlich. Lediglich auf die Gruppierung kann in
Cypher verzichtet werden, da diese automatisch erzeugt wird. Die SQL Abfrage
wird in 58 Millisekunden durchgeführt und ist damit um den Faktor 4 schneller
als die Cypher Abfrage, welche durchschnittlich 232 Millisekunden für die
Durchführung benötigt.

#### Zeige die Summe der 100 meistgenannten Tags an

SQL Abfrage 
~~~roomsql
SELECT SUM([count]) as Summe  
FROM (SELECT TOP 100 profile_mentioned, [count]  
FROM Mentions ORDER BY [count] DESC) as a
~~~

Cypher Abfrage 
~~~roomsql
MATCH ()-[x:MENTIONED]->()  
WITH x.ProfileTag as Profile, x.Count as Amount  
ORDER BY Amount DESC LIMIT 100 RETURN SUM(Amount)
~~~

Die Geschwindigkeit der Kombination Zählen und Summieren wird im SQL Server
20-fach schneller als in Neo4J durchgeführt. Die SQL Abfrage ist nach 10 ms
erfolgreich beendet, die Cypher Abfrage erst nach 236 ms. Die Abfragen selbst
sind sich sehr ähnlich.

#### Zeige die Zeichenanzahl des längsten Beitrags an

SQL Abfrage 
~~~roomsql
SELECT max(len(postcontent)) as PostLength FROM Posts
~~~

Cypher Abfrage 
~~~roomsql
MATCH (n:POST) RETURN max(size(n.PostContent))
~~~

Die Kombination der Aggregatfunktion MAX und LEN ist in SQL ebenfalls schneller.
Mit 38 ms ist die Geschwindigkeit um den Faktor 8 höher als die benötigten 328
ms in Cypher. Die Komplexität ist dagegen sehr ähnlich.

Alle Aggregatfunktionen können, wie in der Einführung zum Thema schon
beschrieben und durch diese Tests bestätigt, mit SQL schneller zu Ende geführt
als mit Cypher.

### Verwendung eines Indexes

Im Folgenden wird die Auswirkung eines Indexes in beiden Datenbanken überprüft.
Die bereits durchgeführte Abfrage nach dem Substring „saw“ in den Beitragstexten
wird nach der Erstellung eines Indexes für diese Spalte/Eigenschaft wiederholt
und verglichen.

#### Anlegen der Indexe

SQL Abfrage 
~~~roomsql
CREATE INDEX postcontentindex on Posts (externalpostid) INCLUDE (postcontent)
~~~

Cypher Abfrage 
~~~roomsql
CREATE INDEX ON :POST(PostContent)
~~~

Die Abfrage in Cypher ist kürzer und leichter verständlich als die SQL Abfrage.

#### Abfragen

SQL Abfrage 
~~~roomsql
SELECT COUNT(*) FROM Posts WHERE postcontent like '%saw%'
~~~

Cypher Abfrage (a) 
~~~roomsql
MATCH (a:POST)  
WHERE toLower(a.PostContent) contains 'saw'  
RETURN count(a)
~~~

Cypher Abfrage (b) 
~~~roomsql
MATCH (a:POST)  
WHERE a.PostContent contains 'saw'  
OR a.PostContent contains 'Saw'  
OR a.PostContent contains 'SAW'  
RETURN count(a)
~~~

Die Dauer der Abfrage im SQL Server hat sich durch den Index um 15% verkürzt.

Die *TOLOWER()* verwendende Abfrage hat keine Geschwindigkeitssteigerungen
erfahren. In der Neo4j Cypher Refcard (Version 3.3) ist dazu vermerkt, das
*TOLOWER()* den Index nicht verwendet. Dies kann durch den Test bestätigt
werden. Die zweite Cypherabfrage kann durch den Index in 363 ms statt  
900 ms durchgeführt werden, dies bedeutet eine Verkürzung der Dauer von ungefähr
60%. Die Cypher Abfrage ist damit auch mehr als doppelt so schnell wie die SQL
Abfrage.

Kombinierte Abfrage
-------------------

Der nächste Block beschäftigt sich zusätzlich zum Vergleich der Abfragezwischen
SQL und Cypher damit, inwieweit eine kombinierte Abfrage zweigeteilt oder in
einem Durchgang mit höherer Performanz durchlaufen werden kann.

### Suche die 10 Person mit den meisten Posts

SQL Abfrage 
~~~roomsql
SELECT TOP 10 profilename, count(*) as Anzahl  
FROM Publishes GROUP BY profilename ORDER BY Anzahl DESC
~~~

Cypher Abfrage 
~~~roomsql
MATCH ()-[pub:PUBLISHED]->(post)  
RETURN pub.ProfileName, count(post) as PostsPublished  
ORDER BY PostsPublished DESC LIMIT 10
~~~

Die Abfrage unterscheidet sich lediglich durch die Art, wie in Cypher eine
Relation abgefragt wird. Die Abfrage in SQL benötigt 75 ms, mit Cypher dauert es
279 ms bis zum Ergebnis.

### Suche die 10 Personen, die am häufigsten genannt wurden

SQL Abfrage 
~~~roomsql
SELECT TOP 10 profile_mentioned, sum([Count]) as Mentioned  
FROM Mentions WHERE mentionedprofileid IS NOT NULL  
AND mentionedprofileid != externalprofileid  
GROUP BY profile_mentioned ORDER BY Mentioned DESC
~~~

Cypher Abfrage 
~~~roomsql
MATCH (q:PROFILE)-[x:MENTIONED]->(p:PROFILE)  
WHERE q <> p  
RETURN p.Names[0] as Name, sum(x.Count) as TimesMentioned  
ORDER BY TimesMentioned DESC LIMIT 10
~~~

Bei der Abfrage nach den am häufigsten genannten Profilen ist ebenfalls SQL
schneller. Mit 12 ms um den Faktor 9 schneller als die gleiche Abfrage in
Cypher.

### Suche die 10 Personen mit dem größten Mentioned/Published Ratio (ohne eigene Mentions)

SQL Abfrage 
~~~roomsql
SELECT profilename, hasPublished, isMentioned, CAST(isMentioned AS float)/CAST(hasPublished as float) as Ratio 
FROM (SELECT externalprofileid, profilename, count(*) as hasPublished  
        FROM Publishes  
        GROUP BY externalprofileid, profilename) as pubs  
    JOIN  
    (SELECT mentionedprofileid, sum([Count]) as isMentioned  
        FROM Mentions WHERE mentionedprofileid IS NOT NULL  
        AND mentionedprofileid != externalprofileid  
        GROUP BY mentionedprofileid) as mens  
    ON pubs.externalprofileid = mens.mentionedprofileid  
ORDER BY Ratio DESC
~~~

Cypher Abfrage 
~~~roomsql
MATCH (pr0:PROFILE)-[men:MENTIONED]->(pr1:PROFILE)-[pub:PUBLISHED]->()  
WHERE pr0 <> pr1  
WITH pr1.Names[0] as Name, count(pub) as hasPublished, sum(men.Count) as isMentioned  
RETURN Name, hasPublished, isMentioned, toFloat(isMentioned)/toFloat(hasPublished) as Ratio  
ORDER BY Ratio DESC LIMIT 10
~~~

Die kombinierte Abfrage mit der zusätzlichen Berechnung des Verhältnisses
zwischen Menge der Erwähnungen und Menge der eigenen Veröffentlichungen wird mit
SQL in der gleichen Dauer beendet wie die Summe der beiden Einzelabfragen. In
Cypher ist dies nicht der Fall, die kombinierte Abfrage benötigt
durchschnittlich 5,5 Sekunden und ist damit ungefähr 14x langsamer als die
Ausführung der Summe beider Einzelabfragen.

Graph Abfragen
--------------

Im Folgenden werden Abfragen auf den Graph direkt gemacht und durch den
Neo4J-Browser auch visuell aufbereitet. Diese Funktionalität ist im SSMS nicht
vorhanden, dennoch werden hier die Zeiten und die Komplexität der Anfragen
verglichen.

![](../../../Desktop/Graphenorientierte%20vs%20relationale%20Datenbank%20-%20Vergleich%20der%20Speicherung%20und%20Abfrage%20bei%20der%20Analyse%20von%20Social%20Data/media/10-konversationen-zweier-profile.png)

Abbildung 10: Konversation mit zwei sich gegenseitig erwähnenden Profilen

### Konversationen finden

SQL Abfrage 
~~~roomsql
SELECT TOP 25 m1.externalprofileid, 
        m1.[Count] as TimesMentioned_12, m2.externalprofileid, 
        m2.[Count] as TimesMentioned_23, m3.externalprofileid
FROM Mentions m1 
    JOIN Mentions m2 
        ON m1.mentionedprofileid = m2.externalprofileid
        AND m1.externalprofileid != m2.externalprofileid
    JOIN Mentions m3
        ON m2.mentionedprofileid = m3.externalprofileid  
        AND m2.externalprofileid != m3.externalprofileid  
        AND m1.externalprofileid = m3.externalprofileid
GROUP BY m1.externalprofileid, m2.externalprofileid, m3.externalprofileid, m1.[Count], m2.[Count]
~~~

![](../../../Desktop/Graphenorientierte%20vs%20relationale%20Datenbank%20-%20Vergleich%20der%20Speicherung%20und%20Abfrage%20bei%20der%20Analyse%20von%20Social%20Data/media/11-konversationen-der-sql-abfrage-in-tabellenform.png)

Abbildung 11: Konversationen der SQL Abfrage in Tabellenform

Cypher Abfrage 
~~~roomsql
MATCH p = (u1:PROFILE)-[:MENTIONED]->(u2:PROFILE)-[:MENTIONED]->(u1:PROFILE)
WHERE u1<>u2
AND NOT (u1:PROFILE)-[:MENTIONED]->(u1:PROFILE)
AND NOT (u2:PROFILE)-[:MENTIONED]->(u2:PROFILE)
RETURN p LIMIT 25
~~~

Die Abfrage in Cypher ist durch die Verwendung des Pfades p im Vergleich zur SQL
Abfrage sehr verkürzt und erlaubt aufgrund der geringeren Duplikationen weniger
Fehler bei der Bearbeitung. Auch die Ausführungszeit ist in diesem Beispiel sehr
unterschiedlich. Die SQL Abfrage benötigt 92 Sekunden bis zum Ende der
Durchführung, die Cypher Abfrage ist bereits nach 10 Millisekunden beendet und
somit 9200-fach schneller.

### Profile, die sich selbst im Beitrag nennen

![](../../../Desktop/Graphenorientierte%20vs%20relationale%20Datenbank%20-%20Vergleich%20der%20Speicherung%20und%20Abfrage%20bei%20der%20Analyse%20von%20Social%20Data/media/12-profil-mit-selbsterwaehnung.png)

Abbildung 12: Profil mit Selbsterwähnung

SQL Abfrage 
~~~roomsql
SELECT TOP 25 profile_mentioned, [Count]  
FROM Mentions  
WHERE externalprofileid = mentionedprofileid  
ORDER BY [Count] DESC
~~~

Cypher Abfrage 
~~~roomsql
MATCH p = (u:PROFILE)-[x:MENTIONED]->(u:PROFILE)  
RETURN p  
ORDER BY x.Count DESC LIMIT 25
~~~

Die Cypher Abfrage verwendet, ähnlich zum vorigen Beispiel, einen Pfad um dem
Muster zu entsprechen. Die SQL Abfrage kann sehr vereinfacht werden, da alle
benötigten Informationen in der Tabelle Mentions ohne *JOINS* abgefragt werden
können. Dadurch dauert die Abfrage in SQL lediglich 6 ms im Gegensatz zur Cypher
Abfrage, die 142 Millisekunden benötigt.

### Profile im Umfeld eines Profiles

Als Startprofil wird das Profil mit der ID *588451086* ausgewählt, der Autor hat
eine übersichtlich Anzahl an 23 Beiträgen auf Twitter erstellt und dabei 88
Personen erwähnt. Es werden alle Profile gesucht, die ausgehend vom Initial
Profil erwähnt werden.

#### Anzeige aller Profile innerhalb einer Distanz von 2

![](../../../Desktop/Graphenorientierte%20vs%20relationale%20Datenbank%20-%20Vergleich%20der%20Speicherung%20und%20Abfrage%20bei%20der%20Analyse%20von%20Social%20Data/media/13-profile-in-naher-distanz.png)

Abbildung 13: Profile in naher Distanz

SQL Abfrage 
~~~roomsql
SELECT DISTINCT mentionedprofileid as Profil, Distance  
FROM (SELECT mentionedprofileid, 1 as Distance
      FROM Mentions
      WHERE externalprofileid = '588451086') as t1
UNION
     (SELECT m2.mentionedprofileid, 2 as Distance
      FROM Mentions m1 JOIN Mentions m2  
      ON m1.mentionedprofileid = m2.externalprofileid
      WHERE m1.externalprofileid = '588451086')
~~~

Cypher Abfrage 
~~~roomsql
MATCH p = (x:PROFILE {ProfileID: '588451086'})-[:MENTIONED\*0..2]->(y:PROFILE)
RETURN DISTINCT y, length(p)
~~~

#### Anzeige aller Profile innerhalb einer Distanz von 5

SQL Abfrage 
~~~roomsql
Zu betrachten im Anhang
~~~

Cypher Abfrage 
~~~roomsql
MATCH p = (x:PROFILE {ProfileID: '588451086'})-[:MENTIONED\*0..5]->(y:PROFILE)
RETURN DISTINCT y, length(p)
~~~

Die SQL Abfrage ist mit \~ 40 Zeilen zu lang für die Darstellung auf dieser
Seite. Für jede Erhöhung der Distanz wird in der Abfrage ein zusätzliches
*SELECT* Statement mit *UNION* eingebunden. Das SELECT Statement besteht dabei
jeweils aus einem zusätzlichen JOIN. Somit besteht das fünfte zu vereinende
SELECT Statement aus einem fünffachen *JOIN* der Tabelle *Mentions*.

Im Gegensatz dazu ist der Unterschied der Abfrage in Cypher lediglich die zu
durchsuchende Reichweite der Relation *Mentioned* durch 0-5 zu ersetzen.

Die Geschwindigkeit der vielfach verjointen SQL Abfrage ist trotz der Länge mit
136 Millisekunden für die 2er Distanz und 16,5 Sekunden für die 5er Distanz
doppelt so schnell wir die Cypher Abfragen mit 350 Millisekunden und 32,5
Sekunden.

#### Anzeige aller Profile von denen das vorgegebene Profil in maximaler Distanz von 2 ist.

Im Gegensatz zur vorigen Abfrage sind nun alle Profile der Startpunkt und es
wird gesucht, bei welchen dieser Profile in einer vorgegeben Entfernung das
gewählte Profil erwähnt wird.

SQL Abfrage 
~~~roomsql
SELECT DISTINCT externalprofileid as Profil, Distance
FROM
(SELECT externalprofileid, 1 as Distance
FROM Mentions
WHERE mentionedprofileid = '588451086') as t1
UNION
(SELECT m1.externalprofileid, 2 as Distance
FROM Mentions m1
JOIN Mentions m2 ON m1.mentionedprofileid = m2.externalprofileid
WHERE m2.mentionedprofileid = '588451086')
~~~

Cypher Abfrage 
~~~roomsql
MATCH p = (x:PROFILE {ProfileID: '588451086'})<-[:MENTIONED\*0..2]-(y:PROFILE)
RETURN DISTINCT y, length(p)
~~~

#### Anzeige aller Profile von denen das vorgegebene Profil in maximaler Distanz von 5 ist

SQL Abfrage 
~~~roomsql
Zu betrachten im Anhang
~~~

Cypher Abfrage 
~~~roomsql
MATCH p = (x:PROFILE {ProfileID: '588451086'})<-[:MENTIONED\*0..5]-(y:PROFILE)
RETURN DISTINCT y, length(p)
~~~

Die SQL Abfrage für die 5er Distanz ist ebenfalls sehr lang und kann im Anhang
betrachtet werden. Der Unterschied zur Abfrage in die entgegen gesetzte Richtung
ist die Austauschung der *externalprofileid* und der *mentionedprofileid* in
allen Vorkommen. Die Änderung der Cypher Abfrage besteht lediglich aus der
Veränderung der Richtung des Pfeils in der Pfadabfrage.

Die Performanz der SQL Abfrage ist ebenfalls größer als die der Cypher Abfrage.
Die Profile in Distanz 2 können bei der SQL Abfrage in 162 Millisekunden
ermittelt werden, die Profile in Distanz 5 in 17,8 Sekunden. Die Abfrage in
Cypher kann die Profile der Distanz 2 in 400 Millisekunden und die Profile der
Distanz 5 in ca. 50 Sekunden ermitteln. Trotz der hohen Verschachtelung durch
eine hohe Anzahl an JOINS ist die Geschwindigkeit mit SQL mehr als doppelt so
hoch.

##### Navigation

| [Deckblatt](00_title.md) |
[Inhalt](00_toc.md) |
[Einleitung](01_introduction.md) |
[Unterschiede der beiden Datenbanktypen](02_db_differences.md) |
[Anwendungsfälle für Graph Datenbanken](03_graphdb_usecases.md) |
[Datengrundlage Social Media](04_data_basis.md) |
[Herangehensweise](05_method.md) |
[Ergebnisse](06_results.md) |
[Fazit](07_conclusion.md) |
[Literaturverzeichnis](08_references.md) |
[Anhang](09_appendix.md) |