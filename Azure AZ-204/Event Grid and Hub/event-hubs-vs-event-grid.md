# Event Hubs vs Event Grid

This is where the info is from https://www.serverless360.com/blog/azure-event-grid-vs-event-hub

## Event Hubs

Event Hubs is a fully managed PaaS which is a real time data ingestion service that is simple, trusted and scalable. It streams millions of events per seconds from any source to build dynamic data pipelines and immediately respond to business challenges. It's used for **telemetry scenarios** as it's for viewing/analyzing lots of events.

Accepts only endpoints for the ingestion of data.

## Event Grid

Event Grid allows you to easily build applications with even-based architectures. First select the Azure resource you would like to subscribe to, then give the event handler or Webhook endpoint to send the event to. It's used when your application deals with **discrete events**, deals with events but not the data.

e.g. you're shipping a product and you're reacting to things like an item has been pulled from the shelf, an item was brought to postal area, item has been shipped etc. Reacting to real changes in real time is a good use case for Event Grid.

Sends HTTP requests to notify events that happen in publishers.
