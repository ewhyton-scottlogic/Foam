# API Management service

Azure API Management is made up of an API gateway, a management plane and a developer portal. These components are Azure-hosted and fully managed by default.

### API Gateway

This endpoint

- Accepts API calls and routes them to appropriate backends
- Verifies API keys and other credentials presented with requests
- Enforces usage quotas and rate limits
- Transforms requests and responses specified in policy statements
- Caches responses to improve response latency and minimize the load on backend services
- Emits logs, metrics and traces for monitoring, reporting and troubleshooting

### Management plane

This is an administrative interface where you set up your API program. Use it to:

- Provision and configure API management service settings
- Define or import API schema
- Package APIs into products
- Set up policies like quotas or transformations on the APIs
- Get insights from analytics
- Manage users

### Developer portal

This is an automatically generated, fully customizable website with the documentation of your APIs. It can:

- Read API documentation
- Call an API via the interactive console
- Create an account and subscribe to get API keys
- Access analytics on their own usage
- Download API definitions
- Manage API keys

## Products

Products in API Management have one or more APIs and are configured with a title, description and terms of use. Products can be opened of protected; protected products must be subscribed to before they can be used; open can be used without a subscription.
Subscription approval is configured at the product level and can either require admin approval or be autoapproved.

## Groups

API Management has the following immutable system groups:

- Administrators (Create API, operations and products that are used by developers, subscription admins are members)
- Developers (Call the operations of the API)
- Guests (Unauthenticated developer portal users, **read-only access**)

Administrators can also create custom groups or use external groups in associated AAD tenants.

## Managed and self-hosted gateways

- Managed: Default gateway component that is deployed in Azure for every API Management instance in every service tier. All API traffic flows through Azure regardless of where backends implementing the APIs are hosted.
- Self-hosted: This is an optional, containerized version of the default managed gateway. Useful from hybrid and multicloud scenarios. Enables customers with hybrid IT infrastructure to manage APIs hosted on-premises and across clouds from a single API Management service in Azure.

## Policies

Note: policies can apply changes to both the inbound request and outbound response.
If you have a policy at the global level and another policy configured for the API then when that particular API is called both policies are applied.

### Advanced Policies

- Control flow - conditionally applies policy statements based on the results of the evaluation of Boolean expressions.
- Forward Request - Forwards the request to the backend service
- Limit Concurrency - Prevents enclosed policies from executing by more than a specified number of requests at a time,
- Log to Event Hub - Sends messages to an Event Hub defined by a Logger entity.
- Mock response - Aborts pipeline execution and returns a mocked response directly to the caller.
- Retry - Retries execution, if and until the condition is met. Execution will repeat at the specified time intervals and up to the specified retry count.

## Secure APIs with Subscriptions

A subscription is a named container for a pair of subscription keys. Devs who need to consume the published APIs can get subscriptions.

| Scope      | Details                                                                                                                                                                                                    |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| All APIs   | Applies to every API accessible from the gateway                                                                                                                                                           |
| Single API | This scope applies to a single imported API and all of its endpoints                                                                                                                                       |
| Product    | A product is a collection of one or more APIs that you configure in API Management. You can assign APIs to more than one product. Products can have different access rules, usage quotas and terms of use. |

You need to pass a subscription key in HTTP requests when they make calls to the API endpoints that are protected by a subscription. Otherwise, you'll get a 401 response.

## Certificates

There are two common ways to verify a certificate:

- Check who issued the certificate. If the issuer was a certificate authority you trust then use it. You can configure trusted certificate authorities in the portal to automate this process.
- If the certificate is issued by the partner, verify that it came from them e.g. they gave it to you in person.

You can check certificates with thumbprints.
