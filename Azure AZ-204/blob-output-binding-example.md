# Blob Output Binding

When doing [[blob-storage-task]] I had to make a blob output binding to write to a blob storage, here is an example of what it means.

```C#
// function name/definition
// function trigger
[Blob("blob-test/trigger", FileAccess.Write, Connection = "AzureWebJobsStorage")] Stream outputFile
```

This is where:

- `"blob-test/trigger"` means the container that we want to write to is called "blob-test" and the file that we are going to make is going to be called "trigger". You must use the syntax container-name/blob-file-name otherwise it gets annoyed.
- `FileAccess.Write` means that we're writing to the file.
- `"AzureWebJobsStorage"` is a reference to the connection string for the storage account that the blob storage container is in, you write the connection string in the `local.settings.json` file in the "Values" object (similar to [[queue-function-trigger-example]] and [[queue-output-binding-example]]).
- `outputFile` is of type Steam which I don't know much about but is something to do with decoding how the file is stored (as it's stored in binary). But this is where we write to, it will be the file we have named in the "blob-test" container (so will be the "trigger" file).
