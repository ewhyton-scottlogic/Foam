### Blob Storage resource types

There are three types of resources

- The **storage account**
- A **container** in the storage account
- A **blob** in the container.

### Storage account

Provides a unique namespace in Azure for your data. Every object in Azure Storage has an address that includes your unique account name. The combination of the two form the base address for the objects in the storage account.

E.g. if storage account is called `myBlobStorage` then the default endpoint for the blob storage is: `http://myBlobStorage.blob.core.windows.net`

### Container in Storage Account

Containers are a set of blobs, similar to a directory in a file system. Storage accounts can include an unlimited number of containers. Container name must be a valid DNS name (as it forms part of a unique URI) and follow these rules

- 3-63 characters long.
- Start with a letter or a number, can only contain lowercase letters, number and a dash -
- Can’t have two or more dashes in a row.

### Blobs

Azure storage supports 3 types of blobs

- **Block blobs** - Text and binary data. Made up of blocks of data that can be managed individually. Store up to 190.7 TiB
- **Append blobs** - made up of blocks like block blobs but optimised for append operations. Ideal for scenarios such as logging data from a VM.
- **Page blobs** - Random access to files up to 8 TB. _Store virtual hard drive_ (VHD) files and serve as disks for VM’s.
