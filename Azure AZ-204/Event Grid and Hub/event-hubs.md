# Azure Event Hubs

Key features of the Event Hubs service

- Fully managed PaaS: little config or management. Integrates with Apache Kafka ecosystems.
- Real-time and batch processing: partitioned consumer model, enabling multiple applications to process the stream concurrently and letting you control the speed of processing
- Capture event data: Azure blob storage or Azure Data Lake storage for retention or micro-batch processing
- Scalable: Auto-inflate (scale throughput units)
- Rich ecosystem.

Key Concepts

- Event Hubs client: primary interface for devs interacting with event hubs client library
- Event Hubs producer: type of client that serves as a source of telemetry data, diagnostics info, usage logs or other log data.
- Event Hubs Consumer: reads info from the Event Hubs and allows processing of it.
- Partition: ordered sequence of events that is held in an Event Hub.
- Consumer group: a view of an entire Event Hubs.
- Event receivers: entity that reads event data from Event Hubs. Via AMQP 1.0
- Throughput units/ processing units.

## Event Hubs Capture

Setting up Capture is fast, there are no admin costs to run it, and it scales automatically in standard or premium tier.
Capture data is written in Apache Avro format: compact, fast, binary format, inline schema.

## Scaling

To scale your event processing application, you can run multiple instances of the application and have it balance the load among themselves. `EventProcessorClient` for .NET and Java and `EventHubConsumerClient` for Python and JavaScript allow you to balance the load between multiple instances of your program and checkpoint events when receiving.

## Control Access to Events

Event Hubs supports both AAD and SAS to handle both authentication and authorization. There are the following build in roles:

- Azure Event Hubs Data Owner: complete access to Event Hubs resources
- Azure Event Hubs Data Sender: send access
- Azure Event Hubs Data Receiver: receiving access

To inspect Event Hubs, you need an `EventHubProducerClient`.
To publish events to an Event Hubs you need to create an `EventHubsProducerClient`.
To read events from an Event Hubs you need to create an `EventHubConsumerClient` for a given consumer group.
It is recommended that the `EventProcessorClient` is used for reading and processing events. Its got a dependency on Azure Storage blobs so you'll need a `BlobContainerClient` for the processor.
