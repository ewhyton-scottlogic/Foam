# Microsoft Graph

In the Microsoft 365 platform, three main components facilitate the access and flow of data:

- Microsoft Graph API offers a single endpoint. You can use REST APIs or SDKs to access the endpoint.
- Microsoft Graph connectors work in the incoming direction, delivering data external to the Microsoft cloud into Microsoft Graph services and applications.
- Microsoft Graph Data Connect provides a set of tools to streamline secure and scalable delivery of Microsoft Graph data to popular Azure data stores.

## Querying the Graph using REST (HTTP)

You need to get authentication tokens (i.e. **be authenticated**) for a user or service in order to make requests to the Microsoft Graph API.

Most resources are defined in the OData namespace in the Microsoft Graph metadata.

To read or write to a resource such as a user or an email message, construct a request that looks like:

```http
{HTTP method} https://graph.microsoft.com/{version}/{resource}?{query-parameters}
```

Where

- {HTTP method}: GET, PUT, POST, PATCH, DELETE (probably GET normally as you'd want to get something from the Graph)
- {version}: version of Microsoft Graph API your application is using (v1.0 or beta)
- {resource}: resource referencing
- {query-parameters}: Optional OData query options or REST method prams that customize the response.

The request back will contain a status code, response message, net link (if the request returns a lot of data you need to page through it by using the URL returned in `@odata.netlink`)

### Resource

Your URL includes the resource you're interacting with in the request such as `me`, user, group, drive and site. Top-level resources often include relationships which you can use to access other resources like `me/messages` or `me/drive`.
You can also interact with resources using methods: e.g. to send an email use `me/sendMail`.

### Query Parameters

These can be OData system query options. You can use them to filter the response for items that match a custom query.
E.g.

```http
GET https://graph.microsoft.com/v1.0/me/messages?filter=emailAddress eq 'jon@contoso.com'
```

This will filter the response for messages that only contain the email address property of jon@sontoso.com

## Query Graph by using SDKs

You will first need to install the Microsoft Graph SDK.
You then need to create a Microsoft Graph client: contains things like tenant ID, client ID and other config things to get authenticated.

### Read information from Microsoft Graph

```c#
// GET https://graph.microsoft.com/v1.0/me

var user = await graphClient.Me
    .Request()
    .GetAsync();
```

List of entries

```c#
// GET https://graph.microsoft.com/v1.0/me/messages?$select=subject,sender&$filter=<some condition>&orderBy=receivedDateTime

var messages = await graphClient.Me.Messages
    .Request()
    .Select(m => new {
        m.Subject,
        m.Sender
    })
    .Filter("<filter condition>")
    .OrderBy("receivedDateTime")
    .GetAsync();
```

You can delete and create new entries and all of the the CRUD operations.

## Best Practices to Microsoft Graph

You need an OAuth 2.0 access token to access data in the Microsoft Graph by either HTTP authorization request header as Bearer token or graph client constructor when using a Microsoft Graph client library. You can also use MSAL to acquire the access token.

### Consent and Authorization

- Use the least privilege you can get away with
- use the correct permission type based on scenarios (delegated if a user is signed in, application if it's a background service)
- consider the end user and admin experience
- consider multi-tenant applications.

Your application should make calls to the Graph and if data is needed to be stored it should only be stored locally, you should also implement proper retention and deletion policies.
