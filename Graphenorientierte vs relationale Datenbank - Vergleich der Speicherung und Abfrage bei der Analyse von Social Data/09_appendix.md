Anhang A – SQL Befehle
======================

Import einer CSV Datei
----------------------
~~~roomsql
CREATE TABLE [dbo].Temporary_Flat_Import (
[postid] [int] PRIMARY KEY,
[externalpostid] [nvarchar](max) NULL,
[contenttype] [nvarchar](30) NULL,
[posttype] [nvarchar](30) NULL,
[posturi] [nvarchar](500) NULL,
[publicationdate] [datetime2](7) NULL,
[externalprofileid] [nvarchar](255) NULL,
[profilename] [nvarchar](100) NULL,
[displayname] [nvarchar](100) NULL,
[profileicon] [nvarchar](255) NULL,
[location] [nvarchar](100) NULL,
[locationlatitude] [nvarchar](100) NULL,
[locationlongitude] [nvarchar](100) NULL,
[locationquadkey] [nvarchar](255) NULL,
[sourceparam] [nvarchar](100) NULL,
[postlanguage] [nvarchar](100) NULL,
[normalscore] [int] NULL,
[provider] [nvarchar](30) NULL,
[providerscore] [float] NULL,
[sentimentvalue] [float] NULL,
[refpost_profilename] [nvarchar](100) NULL,
[refpost_profileid] [int] NULL,
[refpost_profileicon] [nvarchar](255) NULL,
[refpost_profiledisplayname] [nvarchar](100) NULL,
[refpost_externalid] [nvarchar](max) NULL,
[cleanpostcontent] [nvarchar](max) NULL,
[contained_tags] [nvarchar](2047) NULL,
[contained_profiles] [nvarchar](2047) NULL);
~~~

~~~roomsql
BULK INSERT [dbo].Temporary_Flat_Import
FROM '...\\MSE_Mentions_102400.txt'
WITH (FIELDTERMINATOR = '\\t', ROWTERMINATOR = '\\n', FIRSTROW = 2, DATAFILETYPE
= 'widechar')
~~~

Import des Datensatzes
----------------------

Ausgehend vom bereits bestehenden CSV Import (Siehe Anhang A – SQL Befehle:
Import einer CSV Datei)

~~~roomsql
SELECT DISTINCT [externalpostid], [contenttype], [posttype], [posturi],
                [publicationdate], [postlanguage], [sentimentvalue],
                [cleanpostcontent] as postcontent
    INTO [TAXOTest].[dbo].[Posts]
    FROM [TAXOTest].[dbo].[Temporary_Flat_Import]
~~~
~~~roomsql
SELECT DISTINCT [externalprofileid], [sourceparam] as SocialNetwork
    INTO [TAXOTest].[dbo].[Profiles]
    FROM [TAXOTest].[dbo].[Temporary_Flat_Import]
~~~
~~~roomsql
SELECT  [externalpostid], [externalprofileid], [publicationdate],
        [profilename], [displayname], [profileicon],
        [location], [locationlatitude], [locationlongitude]
    INTO [TAXOTest].[dbo].[Publishes]
    FROM [TAXOTest].[dbo].[Temporary_Flat_Import]
~~~
~~~roomsql
SELECT [externalprofileid], [tag], count(tag) as [Count]
    INTO [TAXOTest].[dbo].[Tags]
    FROM (SELECT A.[externalprofileid], Split.a.value('.', 'VARCHAR(500)') AS tag
            FROM (SELECT [externalprofileid],
                    CAST ('\<M\>' + REPLACE([contained_tags],
                    ' ', '\</M\>\<M\>') + '\</M\>' AS XML) AS String
                    FROM [TAXOTest].[dbo].[Temporary_Flat_Import]
                    WHERE contained_tags != '') AS A
            CROSS APPLY String.nodes ('/M') AS Split(a)) as x
    GROUP BY externalprofileid, tag
    ORDER BY [COUNT] DESC
~~~
~~~roomsql
SELECT [externalprofileid], [profile_mentioned], [Count]
    INTO [TAXOTest].[dbo].[temp_Mentions]
    FROM (SELECT externalprofileid, profile as profile_mentioned, count(profile) as [Count]
            FROM (SELECT A.[externalprofileid], Split.a.value('.', 'VARCHAR(500)') AS profile
                    FROM (SELECT [externalprofileid],
                            CAST ('\<M\>' + REPLACE([contained_profiles],
                            ' ', '\</M\>\<M\>') + '\</M\>' AS XML) AS String
                            FROM [TAXOTest].[dbo].[Temporary_Flat_Import]
                            WHERE contained_profiles != ''
                            AND contained_profiles != '\@' ) AS A
                    CROSS APPLY String.nodes ('/M') AS Split(a)) as x
            GROUP BY externalprofileid, profile) as y
            WHERE LEFT(profile_mentioned, 1) = '\@' 
            AND LEN(profile_mentioned) > 1 
            ORDER BY profile_mentioned
~~~

Anzeige aller Profile innerhalb einer Distanz von 5
---------------------------------------------------

~~~roomsql
SELECT DISTINCT mentionedprofileid as Profil, Distance
FROM
    (SELECT mentionedprofileid, 1 as Distance
        FROM Mentions
        WHERE externalprofileid = '588451086') as t1
    UNION
    (SELECT m2.mentionedprofileid, 2 as Distance
        FROM Mentions m1 JOIN Mentions m2
        ON m1.mentionedprofileid = m2.externalprofileid
        WHERE m1.externalprofileid = '588451086')
    UNION
    (SELECT m3.mentionedprofileid, 3 as Distance
        FROM Mentions m1
        JOIN Mentions m2
        ON m1.mentionedprofileid = m2.externalprofileid
        JOIN Mentions m3
        ON m2.mentionedprofileid = m3.externalprofileid
        WHERE m1.externalprofileid = '588451086')
    UNION
    (SELECT m4.mentionedprofileid, 4 as Distance
        FROM Mentions m1
        JOIN Mentions m2
        ON m1.mentionedprofileid = m2.externalprofileid
        JOIN Mentions m3
        ON m2.mentionedprofileid = m3.externalprofileid
        JOIN Mentions m4
        ON m3.mentionedprofileid = m4.externalprofileid
        WHERE m1.externalprofileid = '588451086')
    UNION
        (SELECT m5.mentionedprofileid, 5 as Distance
        FROM Mentions m1
        JOIN Mentions m2
        ON m1.mentionedprofileid = m2.externalprofileid
        JOIN Mentions m3
        ON m2.mentionedprofileid = m3.externalprofileid
        JOIN Mentions m4
        ON m3.mentionedprofileid = m4.externalprofileid
        JOIN Mentions m5
        ON m4.mentionedprofileid = m5.externalprofileid
        WHERE m1.externalprofileid = '588451086')
~~~

Anzeige aller Profile von denen das vorgegebene Profil in maximaler Distanz von 5 ist
-------------------------------------------------------------------------------------

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
    UNION
    (SELECT m1.externalprofileid, 3 as Distance
        FROM Mentions m1
        JOIN Mentions m2 ON m1.mentionedprofileid = m2.externalprofileid
        JOIN Mentions m3 ON m2.mentionedprofileid = m3.externalprofileid
        WHERE m3.mentionedprofileid = '588451086')
    UNION
    (SELECT m1.externalprofileid, 4 as Distance
        FROM Mentions m1
        JOIN Mentions m2 ON m1.mentionedprofileid = m2.externalprofileid
        JOIN Mentions m3 ON m2.mentionedprofileid = m3.externalprofileid
        JOIN Mentions m4 ON m3.mentionedprofileid = m4.externalprofileid
        WHERE m4.mentionedprofileid = '588451086')
    UNION
    (SELECT m1.externalprofileid, 5 as Distance
        FROM Mentions m1
        JOIN Mentions m2 ON m1.mentionedprofileid = m2.externalprofileid
        JOIN Mentions m3 ON m2.mentionedprofileid = m3.externalprofileid
        JOIN Mentions m4 ON m3.mentionedprofileid = m4.externalprofileid
        JOIN Mentions m5 ON m4.mentionedprofileid = m5.externalprofileid
        WHERE m5.mentionedprofileid = '588451086')
~~~


Anhang B – Cypher Befehle 
==========================

Import des Datensatzes
----------------------

~~~roomsql
// Q01-IMPORT Posts
USING PERIODIC COMMIT 1000
LOAD CSV WITH HEADERS FROM 'file:///MSE_Mentions_Full.txt' AS line FIELDTERMINATOR '\\t'
CREATE (a:POST {PostId: toString(line.externalpostid),
                ContentType: toString(line.contenttype),
                PostType: toString(line.posttype),
                PostUri: toString(line.posturi),
                PublicationDate: toString(line.publicationdate),
                Language: toString(line.postlanguage),
                PostContent: toString(line.cleanpostcontent),
                Sentiment: toFloat(line.sentimentvalue),
                TagsMentioned: split(line.contained_tags, " "),
                ProfilesMentioned: split(replace(line.contained_profiles,"\@","")," ")
                })
~~~
~~~roomsql
// Q02-IMPORT Profiles
USING PERIODIC COMMIT 1000
LOAD CSV WITH HEADERS FROM 'file:///MSE_Mentions_Copy.txt' AS line FIELDTERMINATOR '\\t'
MERGE (b:PROFILE {  ProfileID: toString(line.externalprofileid),
                    SocialNetwork: toString(line.sourceparam) })
~~~
~~~roomsql
// Q03-CREATE Published 1/4
USING PERIODIC COMMIT 1000
LOAD CSV WITH HEADERS FROM 'file:///MSE_Mentions_Copy.txt' AS line FIELDTERMINATOR '\\t'
MATCH (a:POST { PostId: toString(line.externalpostid)})
MATCH (b:PROFILE {  ProfileID: toString(line.externalprofileid),
                    SocialNetwork: toString(line.sourceparam) })
MERGE (b)-[c:PUBLISHED { PublicationDate: toString(line.publicationdate),
                         ProfileIcon: toString(line.profileicon)}]->(a)
~~~
~~~roomsql
// Q04-CREATE Published 2/4
USING PERIODIC COMMIT 1000
LOAD CSV WITH HEADERS FROM 'file:///MSE_Mentions_Copy.txt' AS line FIELDTERMINATOR '\\t'
WITH line
WHERE line.location IS NOT NULL
MATCH (a:POST { PostId: toString(line.externalpostid)})
MATCH (b:PROFILE {  ProfileID: toString(line.externalprofileid),
                    SocialNetwork: toString(line.sourceparam) })
MATCH (b)-[c:PUBLISHED { PublicationDate: toString(line.publicationdate),
                         ProfileIcon: toString(line.profileicon)}]->(a)
SET c.Country = toString(line.location)
SET c.Latitude = toString(line.locationlatitude)
SET c.Longitude = toString(line.locationlongitude)
~~~
~~~roomsql
// Q05-CREATE Published 3/4
USING PERIODIC COMMIT 1000
LOAD CSV WITH HEADERS FROM 'file:///MSE_Mentions_Copy.txt' AS line FIELDTERMINATOR '\\t'
WITH line
WHERE line.displayname IS NULL
MATCH (a:POST { PostId: toString(line.externalpostid)})
MATCH (b:PROFILE {  ProfileID: toString(line.externalprofileid),
                    SocialNetwork: toString(line.sourceparam) })
MATCH (b)-[c:PUBLISHED { PublicationDate: toString(line.publicationdate),
                         ProfileIcon: toString(line.profileicon)}]->(a)
SET c.ProfileName = toString(line.profilename)
~~~
~~~roomsql
// Q06-CREATE Published 4/4
USING PERIODIC COMMIT 1000
LOAD CSV WITH HEADERS FROM 'file:///MSE_Mentions_Copy.txt' AS line FIELDTERMINATOR '\\t'
WITH line
WHERE line.displayname <> ""
MATCH (a:POST { PostId: toString(line.externalpostid)})
MATCH (b:PROFILE {  ProfileID: toString(line.externalprofileid),
                    SocialNetwork: toString(line.sourceparam) })
MATCH (b)-[c:PUBLISHED { PublicationDate: toString(line.publicationdate),
                         ProfileIcon: toString(line.profileicon)}]->(a)
SET c.DisplayName = toString(line.displayname)
SET c.ProfileName = toString(reverse(split(line.profilename,"\@"))[0])
~~~
~~~roomsql
// Q07-IMPORT Tags
USING PERIODIC COMMIT 1000 
LOAD CSV WITH HEADERS FROM 'file:///MSE_Mentions_Copy.txt' AS line FIELDTERMINATOR '\\t'
FOREACH (value IN split(line.contained_tags, " ") | 
            CREATE (t:TAG {Tag: value}))
~~~
~~~roomsql
// Q08-CREATE Talks_About
USING PERIODIC COMMIT 1000
LOAD CSV WITH HEADERS FROM 'file:///MSE_Mentions_Copy.txt' AS line FIELDTERMINATOR '\\t'
WITH    line.externalprofileid as externalprofileid, 
        line.sourceparam as sourceparam, 
        split(line.contained_tags, " ") as tags
MATCH (b:PROFILE {  ProfileID: toString(externalprofileid),
                    SocialNetwork: toString(sourceparam) })
MATCH (t:TAG) WHERE t.Tag in tags
FOREACH (value IN tags | MERGE (b)-[x:TALKS_ABOUT]->(t)
    ON CREATE SET x.Count = 1
    ON MATCH SET x.Count = x.Count + 1)
~~~
~~~roomsql
// Q09-COLLECT Names to Profile
MATCH (a:PROFILE)-[b:PUBLISHED]->()
WITH a, COLLECT(DISTINCT b.DisplayName) as dispnams, 
        COLLECT(DISTINCT b.ProfileName) as profnams
SET a.Names = dispnams + profnams
~~~
~~~roomsql
// Q10-CREATE Mentioned
MATCH (a:PROFILE)-[:PUBLISHED]-(b:POST), (c:PROFILE)
    WHERE b.ProfilesMentioned IS NOT NULL
WITH a, c, FILTER(n in b.ProfilesMentioned WHERE n in c.Names)[0] as match
    WHERE match <> []
MERGE (a)-[f:MENTIONED {ProfileTag: match}]->(c)
    ON CREATE SET f.Count = 1
    ON MATCH SET f.Count = f.Count + 1
~~~


Anhang C – Vergleichstabellen
=============================

Tabelle 3: Dauer des CSV Imports mit SQL

| n      | Create | T1    | T2    | T3    | Dauer SQL | 1/ns    |
|--------|--------|-------|-------|-------|-----------|---------|
| 200    | 0,020  | 0,010 | 0,014 | 0,007 | 0,030     | 151,667 |
| 400    | 0,020  | 0,020 | 0,027 | 0,020 | 0,042     | 105,833 |
| 800    | 0,020  | 0,033 | 0,033 | 0,030 | 0,052     | 65,000  |
| 1600   | 0,020  | 0,060 | 0,050 | 0,060 | 0,077     | 47,917  |
| 3200   | 0,020  | 0,137 | 0,116 | 0,110 | 0,141     | 44,063  |
| 6400   | 0,020  | 0,280 | 0,206 | 0,187 | 0,244     | 38,177  |
| 12800  | 0,020  | 0,423 | 0,437 | 0,420 | 0,447     | 34,896  |
| 25600  | 0,020  | 0,773 | 0,797 | 0,810 | 0,813     | 31,771  |
| 51200  | 0,020  | 1,694 | 1,477 | 1,537 | 1,589     | 31,042  |
| 102400 | 0,020  | 3,763 | 3,220 | 3,107 | 3,383     | 33,040  |

Tabelle 4: Dauer des CSV Imports mit Cypher

| n      | T1     | T2     | T3     | Dauer CYP | 1/ns    |
|--------|--------|--------|--------|-----------|---------|
| 200    | 0,139  | 0,130  | 0,131  | 0,133     | 666,667 |
| 400    | 0,204  | 0,160  | 0,164  | 0,176     | 440,000 |
| 800    | 0,219  | 0,231  | 0,219  | 0,223     | 278,750 |
| 1600   | 0,289  | 0,289  | 0,295  | 0,291     | 181,875 |
| 3200   | 0,412  | 0,378  | 0,407  | 0,399     | 124,688 |
| 6400   | 0,628  | 0,635  | 0,739  | 0,667     | 104,271 |
| 12800  | 1,228  | 1,143  | 1,255  | 1,209     | 94,427  |
| 25600  | 2,508  | 2,510  | 2,452  | 2,490     | 97,266  |
| 51200  | 4,919  | 4,874  | 4,853  | 4,882     | 95,352  |
| 102400 | 12,532 | 10,244 | 10,018 | 10,931    | 106,751 |

Tabelle 5: Dauer für das Zählen

|                   | SQL in s |        |       |           | Cypher in s |       |       |           |
|-------------------|----------|--------|-------|-----------|-------------|-------|-------|-----------|
|                   | T1       | T2     | T3    | Ø Dauer   | T1          | T2    | T3    | Ø Dauer   |
| Zähle alle Posts  | 0,016    | 0,024  | 0,023 | **0,021** | 0,002       | 0,001 | 0,001 | **0,001** |
| Zähle IMAGE Posts | 0,040    | 0,037  | 0,040 | **0,039** | 0,114       | 0,108 | 0,107 | **0,110** |
| Zähle 'saw' Posts | 0,953    | 0,920  | 1,050 | **0,974** | 0,907       | 0,753 | 0,745 | **0,802** |
|                   |          |        |       |           | 0,880       | 0,983 | 0,838 | **0,900** |

Tabelle 6: Dauer für das Anzeigen

|                        | SQL in s |        |       |           | Cypher in s |       |       |           |
|------------------------|----------|--------|-------|-----------|-------------|-------|-------|-----------|
|                        | T1       | T2     | T3    | Ø Dauer   | T1          | T2    | T3    | Ø Dauer   |
| Zeige 1.000 Beiträge   | 0,103    | 0,106  | 0,110 | **0,106** | 0,075       | 0,040 | 0,055 | **0,057** |
| Zeige 10.000 Beiträge  | 0,257    | 0,293  | 0,257 | **0,269** | 0,171       | 0,178 | 0,165 | **0,171** |
| Zeige 100.000 Beiträge | 2,086    | 2,160  | 2,124 | **2,123** | 1,582       | 1,605 | 1,495 | **1,561** |

Tabelle 7: Dauer für Aggregatfunktionen

|                       | SQL in s |       |       |           | Cypher in s |       |       |           |
|-----------------------|----------|-------|-------|-----------|-------------|-------|-------|-----------|
|                       | T1       | T2    | T3    | Ø Dauer   | T1          | T2    | T3    | Ø Dauer   |
| Ø Sentiments / Type   | 0,067    | 0,036 | 0,070 | **0,058** | 0,251       | 0,225 | 0,221 | **0,232** |
| Summe top Mentions    | 0,013    | 0,007 | 0,010 | **0,010** | 0,253       | 0,228 | 0,227 | **0,236** |
| Längste Beitragslänge | 0,037    | 0,040 | 0,036 | **0,038** | 0,362       | 0,320 | 0,303 | **0,328** |

Tabelle 8: Unterschiede der Dauer mit Index

|                          | SQL in s |        |       |           | Cypher in s |       |       |           |
|--------------------------|----------|--------|-------|-----------|-------------|-------|-------|-----------|
|                          | T1       | T2     | T3    | Ø Dauer   | T1          | T2    | T3    | Ø Dauer   |
| Zähle 'saw' Posts (Wdh.) | 0,953    | 0,920  | 1,050 | **0,974** | 0,907       | 0,753 | 0,745 | **0,802** |
|                          |          |        |       |           | 0,880       | 0,983 | 0,838 | **0,900** |
| Erstelle Index           | 1,850    | 0,169  |       |           |             |       |       |           |
| Zähle 'saw' Posts        | 0,923    | 0,740  | 0,790 | **0,818** | 0,896       | 0,702 | 0,719 | **0,772** |
|                          |          |        |       |           | 0,382       | 0,348 | 0,359 | **0,363** |

Tabelle 9: Kombinierte Abfragen

|                | SQL in s |        |       |       | Cypher in s |       |       |       |
|----------------|----------|--------|-------|-------|-------------|-------|-------|-------|
|                | T1       | T2     | T3    | Dauer | T1          | T2    | T3    | Dauer |
| Top10 Posts    | 0,066    | 0,073  | 0,087 | 0,075 | 0,291       | 0,27  | 0,277 | 0,279 |
| Top10 Mentions | 0,013    | 0,01   | 0,013 | 0,012 | 0,113       | 0,109 | 0,109 | 0,110 |
| Top10 Ratio    | 0,083    | 0,097  | 0,083 | 0,088 | 5,451       | 5,69  | 5,52  | 5,554 |

Tabelle 10: Graphspezifische Abfragen

|                      | SQL in s |        |        |            | Cypher in s |        |        |            |
|----------------------|----------|--------|--------|------------|-------------|--------|--------|------------|
|                      | T1       | T2     | T3     | Ø Dauer    | T1          | T2     | T3     | Ø Dauer    |
| Top25 Konversations  | 91,530   | 93,584 | 90,540 | **91,885** | 0,011       | 0,01   | 0,01   | **0,010**  |
| Top25 Self Mentioner | 0,003    | 0,007  | 0,007  | **0,006**  | 0,152       | 0,136  | 0,139  | **0,142**  |
| Nachbarn d\<=2       | 0,136    | 0,134  | 0,137  | **0,136**  | 0,36        | 0,36   | 0,32   | **0,347**  |
| Nachbarn d\<=5       | 19,603   | 14,740 | 14,650 | **16,331** | 29,773      | 34,465 | 33,406 | **32,548** |
| istNachbar d\<=2     | 0,177    | 0,156  | 0,154  | **0,162**  | 0,370       | 0,380  | 0,450  | **0,400**  |
| istNachbar d\<=5     | 17,596   | 17,893 | 17,807 | **17,765** | 54,580      | 50,717 | 45,621 | **50,306** |


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
