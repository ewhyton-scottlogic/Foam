# Azure Cosmos DB

Azure Cosmos DB is a fully managed NoSQL database designed for low latency, elastic scalability of throughput, well-defined semantics for data consistency and high availability. It's available in any of the Azure regions.

## Key benefits of global distribution

- Unlimited elastic write and read scalability
- 99.999% read and write availability all over the world.
- Guaranteed reads and writes served in less than 10 ms at the 99th percentile.

Your Azure Cosmos DB account contains a unique DNS name, and you can manage an account by using the Azure portal or the Azure CLI.

You can currently only create a max of 50 Azure Cosmos DB accounts under a single subscription (but you can contact support for this to be raised). After you create an account under your Azure subscription, you can manage the data in your account by creating databases, containers and items.

The hierarchy of different entities in Azure Cosmos DB is Database Accounts → Databases → Containers

## Azure Cosmos DB Containers

A container is horizontally partitioned and then replicated across multiple regions. The items you add to a container are automatically grouped into logical partitions, which are distributed across physical partitions based on the partition key. When you create a container, you configure throughput in one of the following modes:

- Dedicated provisioned throughput mode
- Shared provisioned throughput mode

A container is a schema-agnostic container of items.

Depending on the API, an item can represent either a document in a collection, a row in a table or a node or edge in a graph.

## Consistency Levels

Azure Cosmos DB offers 5 well-defined levels which are:

- Strong.
- Bounded staleness.
- Session.
- Consistent prefix.
- Eventual.

Strong has stronger consistency whereas Eventual has weaker consistency, the further you go up, the higher availability, lower the latency and higher the throughput.

### Strong Consistency

Strong consistency offers a linearizability guarantee where linearizability refers to serving requests concurrently. The reads are guaranteed to return the most recent committed version of an item, a client never sees an uncommitted or partial write and users are always guaranteed to read the latest committed write.

### Bounded Staleness consistency

The reads may lag behind writes by at most "K" versions of an item or by "T" time interval (whichever is first). The minimum value of K=10 write operations and T = 5 seconds for single region accounts. For multi-region accounts the min of K=100,000 and T=300 seconds.

### Session Consistency

Reads are guaranteed to honour the consistent-prefix colour, monotonic reads, monotonic writes, read-your-writes and write-follows-reads guarantees.

### Consistent Prefix Consistency

Writes see eventual consistency. Write operations within a transaction of multiple documents are always visible together.

### Eventual Consistency

There is no ordering guarantee for reads. In the absence of any further writes, the replicas eventually converge. Weakest form of consistency because a client may read the values that are older than the ones it had read before. Ideal for when the application doesn't require any ordering guarantees, e.g. count of Retweets, likes, nonthreaded comments.

The supported API's for MongoDB are written about here [[cosmosDB-supported-api]]

## Request Units

The cost of all database operations (no matter the API) is normalized by Azure Cosmos DB and is expressed by _request units_ or RU's. An RU represents the system resources such as CPU, IOPS and memory that are required to perform the database operations supposed by Cosmos DB. The cost to do a point read (fetching a single item by its ID and partition key value) for a 1 KB item is 1RU. All other database operations (write, point-read, query, upsert, delete) are similarly assigned a cost using RUs.

The type of Azure Cosmos DB account you're using determines the way consumed RUs get charged.

- Provisioned throughput mode: provision the number of RUs on a per-second basis in increments of 100 RUs per second.
- Serverless mode: don't have to provision any throughput when creating resources in Azure Cosmos DB account. At the end of billing period, you get billed for the number of RUs that have been consumed.
- Autoscale mode: automatically scale RUs based on usage. Well suited for mission-critical workloads that have variable or unpredicted traffic patterns.

I then did a task using MongoDB for CosmosDB [[mongodb-for-cosmosdb-spring]].

## Stored procedures

These are JavaScript functions which allow you to create, update, read, query and delete items from inside the Azure CosmosDB container. When you create an item using stored procedures the Cosmos container returns an ID (GUID). The stored procedures are asynchronous and depend on JavaScript function callbacks (if one is missing CosmosDB will throw a runtime error).

Inputs to stored procedures are always sent as a string even if you send and array of strings as an input, it will be converted to a string and passed. You can work around this for example

```JavaScript
function sample(arr) {
    if (typeof arr === "string") arr = JSON.parse(arr);

    arr.forEach(function(a) {
        // do something here
        console.log(a);
    });
}
```

## Triggers and User-Defined functions

CosmosDB supports pre-triggers and post-triggers which you need to register by using Azure Cosmos DB SDK's.
Pre-trigger: before data is sent to the DB, post-trigger: after the data gets back from the DB (e.g. updating metadata).

## Change Feed

Change feed in Azure CosmosDB is a persistent record of changes to a container in the order they occur. It works by listening to a container for any changes then outputs the sorted list of documents that were changed in the order in which they were modified.
You can see inserts and updates in the change feed (you cannot filter the feed for a specific type of document).

There are two types of model, either push model or pull model, Microsoft recommends using the push model because you don't need to worry about polling the change feed for future changed.

### Change Feed Processor

There are four main components of implementing the change feed processor:

- The monitored container (has the data from which the change feed is generated)
- The lease container (acts as state storage and coordinates processing the change feed across multiple workers)
- The compute instance (listens for changes could be a VM, Kubernetes pod, App Service instance, physical machine)
- The delegate (code that defines what you want to do with each batch of changes that the change feed processor reads)
