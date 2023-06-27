# Stored Access Policy

The stored access policy provides an extra level of control over [[shared-access-signature]] on the server side.

The following storage resources support stored access policies:

- Blob containers
- File shares
- Queues
- Tables

## Creating a stored access policy

To create or modify a stored access policy, you need to use the `Set ACL` operation for the resource with a request body that specifies the terms of the access policy.

## Modifying/Revoking a stored access policy

You can modify a stored access policy e.g. you can change it from read-write to read only for all future requests.

You can delete a stored access policy, changing the signed identifier breaks the associations between any existing signatures. Deleting it immediately affects all the SAS associated with it.
