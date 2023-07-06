# Azure Key Vault

Azure Key Vault is a tool for securely storing and accessing secrets. A secret is anything that you want to tightly control access to, such as API keys, passwords, or certificates.

Azure Key Vault supports two types of containers: vaults and managed hardware security module (HSM) pools.
Vaults support storing software and HSM-backed keys, secrets and certificates. Managed HSM pools only support HSM-backed keys.

Azure Key Vault helps solve the following problems:

- Secrets Management (tokens, passwords, certificates, API keys, other secrets)
- Key Management (create and control the encryption keys)
- Certificate Management

There are two service tiers for Key Vault: Standard which encrypts with a software key and Premium which includes HSM-protected keys.

## Key Benefits of using Azure Key Vault

- Centralized application secrets.
- Securely store secrets and keys. (Authentication is done via AAD)
- Monitor access and use.
- Simplified administration of application secrets.

## Azure Key Vault best practices

To do any operations with Key Vault, you will need to authenticate it first. There are three ways to authenticate to Key Vault:

- [[managed-identities]] for Azure resources (recommended as best practice).
- Service principal and certificate (this is not recommended)
- Service principal and secret. (this is not recommended)

Azure Key Vault enforces Transport Layer Security (TLS) protocol to protect data when it's traveling between Key Vault and clients.

The best practices are:

- User separate key vaults for different environments
- Control access to your vault
- Backup (create regular backups of your vault)
- Logging (be sure to turn on logging)
- Recovery options (Tun on soft-delete and purge protection if you want to guard against force deletion of the secret).

## Authenticate to Azure Key Vault

There are two ways to obtain a service principal:

- Enable a system-assigned **managed identity** for the application, this is recommended [[managed-identities]].
- If you can't use managed identity, register the application with your Azure AD tenant.

You can Authenticate to the Key Vault using SDK (.NET, Java, Python, JavaScript) or by using a REST request (HTTP).

If the access token isn't supplied or accepted by the service, you will get a 401 error in the WWW-Authenticate header.
