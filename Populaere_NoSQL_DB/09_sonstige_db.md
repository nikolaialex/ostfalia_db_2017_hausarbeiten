# 7 Sonstige NoSQL Datenbanken

In den abgerufenen Ranking-Listen der NoSQL Datenbankmodelle finden sich verschiedene Einträge von Multi-Model Systemen, beispielsweise sind allein 7 der ersten 10 Graphendatenbanken als Multi-Model ausgewiesen.


## 7.1 Multi-Model Datenbanken
In den abgerufenen Ranking-Listen der NoSQL Datenbankmodelle finden sich verschiedene Einträge von Multi-Model Systemen, beispielsweise sind allein 7 der ersten 10 Graphendatenbanken als Multi-Model ausgewiesen.


|   Multi-Model DDMS   |   Wide-Column   |   Key-Value   |   Document   |   Graph   |   
|   -----   |    ------   | ------   | ------   | ------   |   
|   Amazon DynamoDB    |      |   ✔  |  ✔   |  
|   MS Azure Cosmos DB   |   ✔   |   ✔  |  ✔   |  ✔  |   
|   OrientDB   |      |   ✔  |  ✔   |  ✔  |  
|    ArangoDB  |     |   ✔  |  ✔   |  ✔  | 
Tabelle 7.1 Kombinationen von Datenbankmodellen

Dabei fällt insbesondere das proprietäre Microsoft System Azure Cosmos DB auf, welches den Versuch unternimmt, die vier zuvor betrachteten NoSQL Datenbankmodelle "unter einen Hut" zu bekommen. Azure Cosmos DB ist eine Multi-Model NoSQL-DB von Microsoft, die aus der schemalosen Azure DocumentDB hervorgegangen ist. 

Cosmos DB kann für verschiedene Einsatzzwecke unterschiedlich optimiert werden im Bezug Nenngrößen wie Durchsatz, Speicherplatz und Konsistenz. Standardmäßig werden Daten bereits beim Hinzufügen automatisch indiziert, dadurch werden Daten-Abfragen in den unterschiedlichsten Abfragesprachen möglich sind.

Ein weiteres Merkmal von Cosmos DB ist die Integration einer JavaScript-Engine in die Datenbank, wodurch in natürlicher Weise JSON-Dokumente gespeichert und abgefragt werden können. Darüber hinaus bringt CosmosDB die aus der relationalen Welt bekannten Konzepte von Stored Procedures und Triggern mit, mit dem eine ACID-konforme Sequenz von DB-Manipulationen möglich ist. Somit kann Cosmos DB sowohl mit SQL-Dialekten als aus mit LINQ, JavaScript und der MongoDB-Query-Language abgefragt werden.

Microsoft nennt als Einsatzgebiet für die Cosmos DB "managing data at planet-scale", was auf den Umstand aufmerksam machen soll dass die Cosmos DB ihre Daten über "partitioned collections" selbstständig über verschiedene DB-Knoten hinweg gemäß den jeweiligen Platz- und Abfrage-Bedürfnissen verteilen kann.


***

Nächstes Kapitel: [8. Fazit][kap8]  

[kap8]:             ./8_fazit.md "Fazit"
