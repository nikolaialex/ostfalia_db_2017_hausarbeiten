# 2. Grundlagen und Szenario
//was sind cloud based Datenbanken - Anforderungen
//Aufbau von DynamoDB - Funktionsweise?
   -Konkreter für die Performance: (Client -> dynamodb Accelorator (Cache) -> dynamodb)
//welches Reallife-Szenarien gibt es

## Mongo DB
![Logo Mongo DB](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/images/mongodb-logo.png "Logo Mongo DB")

## Amazone Dynamo DB
![Logo Dynamo DB](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/images/aws-dynamodb.png "Logo Dynamo DB")

Quelle: https://engineering.opsgenie.com/crossing-the-river-an-adventure-of-cross-region-replication-with-dynamodb-d0b5d72645bb
> #### AWS DynamoDB in Short
> DynamoDB is a fully managed NoSQL database by AWS. It has a scheme-less architecture and supports both Document Data Model and Key-Value Data Model. To achieve high uptime and durability, DynamoDB supports automatic and synchronous data replication across three facilities (Availability Zones) in a region. This is to protect your data against individual node or even facility level failures. Although this is a great built-in feature to mitigate zone based failures, this will not work in case of a regional failure. There is no built-in protection for regional failures, at least for now. However, in mission-critical systems, regional failures should be handled in order to have a robust system which also should be highly available, fault-tolerant and resilient. Cross-region replication can be a good solution to handle regional failures when using DynamoDB.


----------

[vorheriges Kapitel](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/1_Einleitung.md)
   |   [nächstes Kapitel](https://github.com/kuzdu/DBS---DynamboDB-vs-MongoDB/blob/master/3_PerformanceUntersuchen.md)