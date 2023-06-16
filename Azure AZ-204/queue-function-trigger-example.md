# Queue Function Trigger

When doing [[blob-storage-task]] I had to make a queue function trigger. This is the example of my queue trigger, you get most of this when you use the template for a queue trigger.

```C#
public static async Task Run( // <- if you use an async method then even if the method is void use return Task (with no return in the method)
    [QueueTrigger("output", Connection = "AzureWebJobsStorage")]string myQueueItem)
```

This is where:

- `"output"` is the queue that you are listening to/ waiting for something to be written to
- `"AzureWebJobsStorage"` similar to [[queue-output-binding-example]] and [[blob-output-binding-example]], is a reference to the connection string for the storage account that the queue is in. You need to define it in the `local.settings.json` file in the "Values" object.
- `myQueueItem` is the contents of the queue which we know is a string.
