# Azure Blob Storage

Note: this note is mostly on Google sheets and a link can be found [here](https://docs.google.com/document/d/13NNbA-W6GsHErmJqHf349SViWEoc2JSskKj1LAJ1Dl0/edit).

Blob (Binary Large Object) storage is optimised for storing massive amounts of unstructured data (text or binary data).

Blob storage is designed for:

- Serving images or documents directly to the web browser
- Storing files for distributed access
- Streaming video or audio
- Writing to log files
- Storing data for backups, disaster recovery, archiving
- Storing data for analysis for an on-premise or Azure hosted service.

Objects in blob storage are accessible by the Azure Storage REST API, Azure PowerShell, Azure CLI, or an Azure storage client library.
Blob storage is accessible all around the world via HTTP or HTTPS.

There are two storage levels, standard or premium (which uses SSD's).

| Storage account type | Performance tier | Usage                                                                                              |
| -------------------- | ---------------- | -------------------------------------------------------------------------------------------------- |
| General purpose v2   | Standard         | Blobs, file shares, queues and tables. Recommended for most scenarios. Supports lifecycle policies |
| Block blob           | Premium          | Block blobs and append blobs. Recommended for high transaction rates with low latency              |
| Page blobs           | Premium          | Page blobs only                                                                                    |

There are different [[blob-storage-access-tiers]] and [[blob-storage-resource-types]] which are key to remember!

## Manage Container Properties and metadata by using .NET

System properties: These exist on each Blob storage resource. Some can be red or set, while others are read-only, and they correspond to certain HTTP headers.

User-defined metadata This consists of one or more name-value pairs that you specify for a Blob storage resource. They are for your own purposes only and don't affect how the resource behaves.

Metadata name/value pairs are valid HTTP headers, so must be **valid HTTP header names** and **valid C# identifiers** (ASCII characters, case-insensitive) total size of all metadata pairs can be up to 8 KB in size.
Metadata has GET, HEAD and PUT operations.

### Set and retrieve metadata

To set metadata, add name-value pairs to an `IDictionary` object, and then call one of the following methods of the `BlobContainerClient` class to write the values `SetMetadata` and `SetMetadataAsync`.
The name of the metadata must conform to the naming conventions for C# identifiers.

The following code is an example for setting metadata on a container.

```C#
public static async Task AddContainerMetadataAsync(BlobContainerClient container)
{
    try
    {
        IDictionary<string, string> metadata =
           new Dictionary<string, string>();

        // Add some metadata to the container.
        metadata.Add("docType", "textDocuments");
        metadata.Add("category", "guidance");

        // Set the container's metadata.
        await container.SetMetadataAsync(metadata);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"HTTP error code {e.Status}: {e.ErrorCode}");
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

Just as there's `SetMetadata` there's `GetProperties`.

This code retrieves metadata from a container.

```C#
public static async Task ReadContainerMetadataAsync(BlobContainerClient container)
{
    try
    {
        var properties = await container.GetPropertiesAsync();

        // Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in properties.Value.Metadata)
        {
            Console.WriteLine($"\tKey: {metadataItem.Key}");
            Console.WriteLine($"\tValue: {metadataItem.Value}");
        }
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"HTTP error code {e.Status}: {e.ErrorCode}");
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

Metadata headers are named with the header prefix `x-ms-meta-` and a custom name.
