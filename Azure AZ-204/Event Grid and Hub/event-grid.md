# Azure Event Grid

Azure Event grid is a serverless event broker that you can use to integrate applications using events. Publishers emit event but don't know how they're going to be handled. Subscribers decide which events they want to handle.

## Concepts in Event Grid

There are 5 key concepts

- Events (what happened) [[event]]
- Event Sources (where the event took place)
- Topics (the endpoint where publishers send events. There are System topics (built in) and custom topics (third party)).
- Event subscriptions (the endpoint to route events, sometimes to more than one handler)
- Event handlers (app or service reacting to the event).

An event is the smallest amount of information that fully describes something that's happened in the system. Every event has information like source of event, time it took place, unique identifier.

There are two types of event schemas that are supported by Event Grid: Cloud event schema and Event Grid event schema

Cloud event supports JSON implementation of CloudEvents v1.0 and HTTP.

Event grid doesn't guarantee order for event delivery, so you may not get things back in the same order that they were sent.

The following table represents the types of endpoint and the error codes which will not trigger a retry (if it's setup):
| Endpoint Type | Error Codes |
|-----------------|-------------------------|
| Azure Resources | 400, 413, 403 |
| Webhook | 400, 413, 403, 404, 401 |

If the error isn't one of the ones above, then it will wait 30 seconds and try again.

## Retry Policy

There are two cofigurations which can be costomised when setting up your retry policy:

- Maximum number of attempts (between 1 and 30 default is 30)
- Event time-to-live (TTL) (in mins between 1 and 1440 default is 1440 mins)

## Output Batching

Batched delivery has two settings:

- Max events per batch (between 1 and 5000)
- Preferred batch size in kilobytes.

## Dead-letter events

When Event Grid can't deliver an event within a certain time period or after trying to deliver the event a number of times, it can send an undelivered event to a storage account. This is called **dead-lettering**. Event Grid dead-letters an event when one of the following conditions is met:

- Event isn't delivered within the time-to-live period.
- Number of tried to deliver the event exceeds the limit.

This isn't turned on by default, so you need to enable a storage account to hold undelivered events.

Event Grid uses RBAC and these are the built-in roles

| Role                     | Description                                              |
| ------------------------ | -------------------------------------------------------- |
| Subscription Reader      | Lets you read Event Grid event subs                      |
| Subscription Contributor | Lets you manage Event Grid event subscription operations |
| Event Grid Contributor   | Lets you create and manage Event Grid resources          |
| Event Grid Data Sender   | Lets you send events to Event Grid topics.               |

## Endpoint Validation with Event Grid Events

If you're using a non webhook endpoint (e.g. HTTP trigger based Azure Function) your endpoint code needs to participate in a validation handshake with Event Grid. There are two ways of validating the subscription

- Synchronous handshake: At the time of the event subscription creation Event Grid sends a subscription validation event to the endpoint which is validated and returns the code synchronously. Supported in all Event Grid versions,
- Asynchronous handshake: used if you're using third-party services like Zapier or IFTTT.

## Filter Events

There are three options for filtering

- Event types
- Subject begins with or ends with
- Advanced fields and operators.
