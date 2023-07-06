# Azure App Configuration Service

Use App Configuration to store all the settings for your application and secure their accesses in one place.

App Configuration has the following benefits:

- A fully managed service that can be set up in mins
- Flexible key representations and mappings
- Tagging with labels
- Point-in-time replay of settings
- Dedicated UI for feature flag management
- Enhanced security through Azure-managed identities
- Encryption of sensitive information at rest and in transit
- Native integration with popular frameworks.

App configuration complements [[key-vault]] but makes it easier to implement

- Centralize management and distribution of hierarchical config data for different environments and geographies.
- Dynamically change application settings without the need to redeploy or restart an application
- Control feature availability in real-time.

App configuration is NOT a replacement for Azure Key Vault, **do not store application secrets in it**.

You can use in .NET core and ASP.NET core, .NET framework and ASP.NET, Java Spring, JavaScript/Node.js Python and much more!

## Keys (key-value pairs)

It's common practice to organise keys into a hierarchical namespace by using a character delimiter, such as `/` or `:`. Keys are **case-sensitive**. You can only use `*`, `,` and `\` by doing `\{character}` because their reserved characters.

Example of hierarchical naming key names :

```
AppName:Service1:ApiEndpoint
AppName:Region2:DbEndpoint
```

You can also give key names a label, by default this label is null (if you don't give it one) e.g.

```
Key = AppName:DbEndpoint & Label = Test
Key = AppName:DbEndpoint & Label = Staging
Key = AppName:DbEndpoint & Label = Production
```

Values assigned to the key are Unicode strings.

## Manage application features

Basic concepts:

- Feature Flag - binary state of _on_ or _off_. Has an associated code block.
- Feature manager - application package that handles the lifecycle of all the feature flags in an application.
- Filter - Rule for evaluating the state of a feature flag.

An effective implication of feature management consists of at least two components working in concert: feature flags, store of the flags current states.

You can implement these very simply in the form of a conditional `if` statement

```java
boolean featureFlag = true;

if (featureFlag) {
    // do something when the flag is true
} else {
    // do something if the flag is false
}
```

## Customer managed keys

When customer-managed key capability is enabled, App configuration uses a managed identity assigned to the App configuration instance to authenticate with AAD. The managed identity then calls Azure Key vault and wraps the App config instance's encryption key. The wrapped encryption key is stored and the unwrapped encryption key is cached within App config for one hour.

To enable customer-managed key capability for App Configuration you need:

- Standard tier Azure App Configuration instance
- Azure Key Vault with soft-delete and purge-protection features enabled.
- An RSA or RSA-HSM key within the Key Vault.

## Using Private endpoints for App Configuration

Using private endpoints for your App Configuration enables you to:

- Secure your application config details by configuring the firewall to block all connections to App Configuration on the public endpoint.
- Increase security for the VNet ensuring data doesn't escape from the VNet.
- Securely connect to the App Configuration store from on-premises networks that connect to the VNet using VPN or ExpressRoutes with private-peering.
