# Managed Identities

Managed Identities eliminate the need for devs to manage their secrets, credentials, certificates and keys used to secure communication between services. The token which is used for Managed identities to gain access to a given resource is a **Service Principal** token.

There are two types of managed identities:

- System-assigned managed identity.
- User-assigned managed identity.

### System-assigned

This is enabled directly on an Azure service instance. If the instance is deleted, Azure automatically cleans up the credentials and the identity in Azure AD.

### User-assigned

Created as a standalone Azure resource. Azure creates an identity in the AD tenant that's trusted by the subscription in use. After it's created the identity can be assigned to one or more Azure service instances. The lifecycle is managed separately from the lifecycle of the Azure service instance to which it's assigned.

The table highlights some of the key differences.

| Characteristic                 | System-assigned managed identity                                                    | User-assigned managed identity                                                                              |
| ------------------------------ | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Creation                       | Created as part of an Azure resource (e.g. VM or App service)                       | Created as a standalone Azure resource                                                                      |
| Lifecycle                      | Shared lifecycle with the Azure resource that the managed identity is created with. | Independent lifecycle. Must be explicitly deleted                                                           |
| Sharing across Azure resources | Can't be shared, only associated with a single Azure resource.                      | Can be shared, the same user-assigned managed identity can be associated with more than one Azure resource. |

## Access Token

The Azure Identity library supports a `DefaultAzureCredential` type. It automatically attempts to authenticate via multiple mechanisms, including environment variables or an interactive sign-in.
These are the following mechanisms:

- Environment
- Managed Identity
- Visual Studio
- Azure CLI (using `az login`)
- Azure PowerShell (using `Connect-AzAccount`)
- Interactive Browser.

Potential exercise:
Create a user-assigned managed identity, a storage account (blob) make a role assignment so the managed identity can be a contributor to the storage account, then make an azure function which uses the managed identity and allows you to write to the storage account.
