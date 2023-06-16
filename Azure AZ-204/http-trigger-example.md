# HTTP Trigger Example

When doing [[blob-storage-task]] I made a variety of triggers and bindings. Here's the example in C# of my HTTP trigger.

Example of HTTP trigger (you get this when you use the template for HTTP trigger):

```C#
public static async Task<IActionResult> Run( // <- Function defnition
[HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, // <- HTTP triggger
ILogger log) // <- I think this helps with logs
```

I think the "get" and "post" are the HTTP CRUD methods that it can take in, `req` is the body of the HTTP request.
