# Untersuchung von cloud-hosted Datenbanken in Bezug auf Performance, Sicherheit und Backup-Strategien am Beispiel MongoDB und Amazone DynamoDB

## Inhaltsverzeichnis

0. Linkliste
1. [Einleitung](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/1_Einleitung.md)
2. [Grundlagen und Szenario](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/2_GrundlagenUndSzenario.md)
3. [Performance Untersuchen](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/3_PerformanceUntersuchen.md)
4. [Sicherheit und Backup Strategien](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/4_SicherheitUndBackupStrategien.md)
    1. Was für Sicherheit ist notwendig? Was möchte man überhaupt sichern?
    2. Welche Sicherheit gewährleistet dynamoDB?
    3. Was für Backup Strategien gibt es?
    4. Welche verwendet dynamoDB?
    5. Wie verwendet dynamoDB diese
5. [Fazit/Schluss](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/5_FazitSchluss.md)
6. [Literaturverzeichnis](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/6_Literaturverzeichnis.md)

Linkliste
---------

Für den Überblick: 
* [Wikipedia Amazon DynamoDB](https://de.wikipedia.org/wiki/Amazon_Dynamo)
* [FAQ von DynamoDB](https://aws.amazon.com/de/dynamodb/faqs/)
* [Amazon Youtube Videos über DynamoDB](https://www.youtube.com/user/AmazonWebServices/search?query=dynamoDB)
* [Alle Whitepaper von AWS](https://aws.amazon.com/de/whitepapers/)
* [DynamoDB versus MongoDB Stackoverflow](https://stackoverflow.com/questions/17931073/dynamodb-vs-mongodb-nosql)
* [DynamoDB better than MongoDB Blog](https://blog.cloudthat.com/5-reasons-why-dynamodb-is-better-than-mongodb/)
* [DynamoDB versus MongoDB New Gen Apps](https://www.newgenapps.com/blog/bid/213202/amazon-dynamodb-vs-mongodb)
* [DynamoDB versus MongoDB OptimalBi](https://optimalbi.com/blog/2017/03/15/dynamodb-vs-mongodb-battle-of-the-nosql-databases/)
* [DynamoDB versus MongoDB MongoDB-Website](https://www.mongodb.com/compare/mongodb-dynamodb)
* [DB Engines DynamoDB und MongoDB ](https://db-engines.com/en/system/Amazon+DynamoDB%3BMongoDB)
* [DB Engines Ranking](https://db-engines.com/en/ranking)
* [Differences DynamoDB MongoDB Quora](https://www.quora.com/What-are-differences-between-MongoDB-and-Amazon-DynamoDB)
* [insights dice Blog Why my Team went with DynamoDB over MongoDB](https://insights.dice.com/2013/02/21/why-my-team-went-with-dynamodb-over-mongodb/)
* [DynamoDB versus MongoDB Slideshare](https://de.slideshare.net/AmarDas8/compare-dynamodb-vs-mongodb)
* [DynamoDB versus MongoDB innovapoint](https://www.innovapoint.com/resources/blogs/dynamodb-vs-mongodb/)
* [DynamoDB Ten Things](https://cloudacademy.com/blog/amazon-dynamodb-ten-things/)
* [Info Tutorial: How to Migrate From hosting MongoDB to DynamoDB](https://auth0.com/blog/how-to-migrate-from-hosting-mongodb-to-dynamodb/)
* [Grenzen DynamoDB: You probably shoudn't use DynamoDB](https://syslog.ravelin.com/you-probably-shouldnt-use-dynamodb-89143c1287ca)

--------
[nächstes Kapitel](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/1_Einleitung.md)