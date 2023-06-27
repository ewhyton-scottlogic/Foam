# Shared Access Signature (SAS)

This is a URI that grants restricted access rights to Azure Storage resources.

There are three types:

1. User delegation SAS - Secured with the AAD, applies to **blob storage only**
2. Service SAS - Secured with the storage account key, blob storage, queue storage, table storage or azure files
3. Account SAS - Secured with the storage account key, for one or more storage service.

Microsoft recommends that you use AAD credentials when possible as a security best practice.
Best practices:

- Always use HTTPS
- Most secure is user delegation, use it wherever possible.
- Try and set expiration time to the smallest useful value.
- Only grant the access that's required.

## When to use Shared Access Signatures

Use a SAS when you want to provide secure access to resources in your storage account to any client who doesn't otherwise have permission to those resources.

SAS is required when:

- You copy a blob to another blob from a different storage account
- When you copy a file to another file that lives in a different storage account
- When you copy a blob to a file, or a file to a blob even if it's within the same storage account.
