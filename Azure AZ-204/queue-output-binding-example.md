# Queue Output Binding Example

When doing [[blob-storage-task]] I had to make a queue output binding in C#, some of the guides online are not helpful so here's what I did.

Example of Queue output binding:

```C#
// function name/definition e.g. public static async Task<IActionResult> Run(
// function trigger
[Queue("output"), StorageAccount("AzureWebJobsStorage")] ICollector<string> outputQueue
```

This is where:

- `"output"` is the name of the queue that you want to write to.
- `"AzureWebJobsStorage"` is a pointer to the connection string for the storage account that the queue you want to write to is. You need to define this in the `local.settings.json` file in a values object similar to [[blob-output-binding-example]] and [[queue-function-trigger-example]].
- `outputQueue` is the queue, so you want to use the `.Add` method on this to add something into the queue e.g. in [[blob-storage-task]] I did `outputQueue.Add("Name passed to the function: " + name);`.
