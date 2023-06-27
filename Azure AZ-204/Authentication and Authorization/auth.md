# Authentication and Authorization

## Microsoft Identity Platform

The Microsoft identity platform helps you build applications your users and customers can sign in to using their Microsoft identities or social accounts, and provide authorized access to your own APIs or Microsoft APIs (e.g. Graph API).

There are several components that make up the Microsoft identity platform:

- OAuth 2.0 and OpenID
- Open-source libraries (MSAL)
- Application management portal
- Application configuration API and PowerShell.

An application must be registered with an Azure Active Directory tenant. When you register an app in the portal you choose whether it's:

- Single tenant: only accessible in your tenant
- Multi-tenant: accessible in other tenants

## Application Object

An AAD application is defined by its one and only application object. It is used as a template or blueprint to create one of more service principal objects. A service principal is created in every tenant where the application is used, it has static properties that are applied to all the created service principals (like a class in java).

It describes

- How the service can issue tokens in order to access the application
- Resources that the application might need to access
- Actions that the application can take.

It has a one to one relationship with the software application and one to many relationship with its corresponding service principal object.

### Service Principal object

There are three types of service principal:

- Application (local representation): service principal defines what the app can actually do in the specific tenant.
- Managed Identity
- Legacy (legacy app created before app registrations)

Must be created in each tenant where the application is used to enable it to establish an identity for sign-in and access to resources being secured by the tenant.

### Relationship between application objects and service principals:

Application object is _global representation_ of your application, service principal is the _local representation_ for use in a specific tenant.

## Permissions and Consent

The Microsoft identity platform implements OAuth 2.0.

In the Microsoft identity platform, a permission is represented as a string value. An app most commonly requests permissions by specifying the scopes in requests to the Microsoft identity platform authorize endpoint. Some high-privilege permissions can be granted only through administrator consent.

### Permission types

There are two types:

- Delegated permissions: used by apps that have a signed-in user present. The app is delegated with the permission to act as a signed-in user when it makes calls to the target resource.
- App-only permissions: used by apps that run without a signed-in user e.g. apps that run as a background service or daemons. Only an admin can consent to app-only access permissions.

### Consent Types

**Static user consent**: You must specify all the permissions it needs in the app's config. It also enables admins to consent on behalf of all users in the organization.
Possible problems

- App needs to request all permissions it would ever need upon user's first sign-in → long list of permissions that discourages end users from approving the app's access on initial sign-in.
- App needs to know all the resources it would ever access ahead of time.

**Incremental and dynamic user consent**: You can ask for a minimum set of permissions upfront and request more overtime as the customer uses more app features. It only applies to delegated permissions and not to app-only access permissions. Can be convenient but presents a big challenge for permissions that require admin consent.

**Admin Consent**: Required when your app needs access to certain high-privilege permissions. Still requires the static permissions registered for the app.

## Conditional Access

Conditional access enables devs and enterprise customers to protect services in a multitude of ways including

- Multifactor authentication
- Allowing only Intune enrolled devices to access specific services
- Restricting user locations and IP ranges.

The following scenarios require code to handle Conditional Access challenges:

- Apps performing the on-behalf-of flow
- Apps accessing multiple services/resources
- Single-page apps using MSAL.js
- Web apps calling a resource.

Examples:
iOS app such that when a policy is invoked the user needs to perform multifactor authentication.

## MSAL (Microsoft Authentication Library)

Is used to provide secure access to Microsoft Graph and other Microsoft APIs, third-party web APIs or your own web API.
MSAL gives you many ways to get tokens and provides the following benefits:

- Don't need to use OAuth libraries or code against the protocol.
- Acquires tokens on behalf of the user/app.
- Maintains a token cache and refreshes tokens for you when they're close to expire.
- Helps troubleshoot your app by exposing actionable exceptions, logging and telemetry.

MSAL.js supports single-page application. MSAL.NET does not support single-page web apps.
Wide range of SKD's and phone operating system support.

Public client app: runs on devices, desktop computers or in web browser. Not trusted to keep application secrets.
Confidential client app: runs on servers. Difficult to access to can keep application secrets.

## Initialize client applications

With MSAL.NET 3.x they recommend using `PublicClientApplicationBuilder` and `ConfidentialClientApplicationBuilder`.

You'll need the following to after registering your app with the Microsoft identity platform:

- Client ID (a string representing a GUID).
- Identity provider URL (instance) and sign in audience (known together as the authority).
- Tenant ID.
- Application secret or certificate.
- `redirectUri` (for web apps).
