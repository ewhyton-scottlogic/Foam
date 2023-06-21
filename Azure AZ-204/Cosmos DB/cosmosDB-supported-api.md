# Supported APIs for MongoDB

Cosmos DB offers support for multiple database APIs including (all start Azure Cosmos DB for):

- NoSQL (**document format**, the best end-to-end experience, support for new features)
- MongoDB (**document structure** via **BSON**, doesn't have native MongoDB related code)
- PostgreSQL (run with Citus open source, single node or multi-node configuration)
- Apache Cassandra (**column-oriented** schema, horizontal scaling, compatible with native Apache Cassandra)
- Table (**key/value format**, improvement to standard Azure Table Storage, recommended to migrate over to this)
- Apache Gremlin (**graph structure**)

API for NoSQL is native to Azure Cosmos DB.
The others are best suited for when

- Already existing MongoDB, PostgreSQL, Cassandra or Gremlin applications.
- Don't want to rewrite your entire data access layer
- Use open-source developer ecosystem, client-drivers, expertise, and resources for your database.
