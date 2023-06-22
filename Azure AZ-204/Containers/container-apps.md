# Azure Container Apps

Azure Container Apps enables you to run microservices and containerized applications on a serverless platform that runs on top of Azure Kubernetes Service. Common uses of Container Apps includes:

- Deploying API endpoints
- Hosting background processing applications
- Handling event-driven processing
- Running microservices.

Apps build on Container Apps can dynamically scale based on HTTP traffic, event-driven processing, CPU or memory load and any KEDA-suppored scalar.

With Container Apps you can

- Run multiple container revisions
- Autoscale apps (scale to zero)
- Enable HTTPS ingress without having to manage other Azure infrastructure.
- Split traffic across multiple versions of an application
- Use internal ingress and service discovery for secure internal-only endpoints with built-in DNS based service discovery.
- Build microservices with [Dapr](https://docs.dapr.io/concepts/overview/)
- Run containers from a registry.
- Provide an existing virtual network when creating an environment for your container apps.
- Securely manage secrets directly in your app.
- Monitor logs using Azure Log analytics.

Container Apps can use any runtime, programming language or development stack of your choice; if a container crashes, it automatically restarts.

## Multiple containers

You can define multiple containers in a single container app to implement the sidecar pattern. The containers in a container app share hard disk and network resources and experience the same application lifecycle.
Note running multiple containers in a single container app is an advance use case. In most situations where you want to run multiple containers such as when implementing a microservice architecture, deploy each service as a separate container app.

### Limitations

Azure Container Apps has the following limitations:

- **Privilege containers**: Container Apps can't run privileged containers. If your program attempts to run a process that requires root access, the app inside the container experiences a runtime error.
- **Operating system**: Linux-based (x64/86) container images are required.

## Authentication and Authorization in Azure Container Apps

Like App services, Container Apps provides built in authentication and authorization features to secure your external ingress-enabled container app with minimal or no code. Container Apps provides access to various built-in authentication providers.
This feature should only be used with HTTPS. To restrict app access to only authenticated users, set its Restrict access setting to **Require Authentication**. To authenticate but not restrict access, set its Restrict access setting to **Allow Unauthenticated**.

The authentication and authorization middleware component is a feature of the platform When enabled, every incoming HTTP request passes through the security layer before being handled by your application. The platform middleware handles several things: Authenticates users and clients, manages authenticated session, injects identity information into HTTP request headers.

### Authentication Flow

- Without provider SDK (server-directed flow) delegates sign-in to container apps and presents the provider's sign-in page to the user.
- With provider SKD (client-directed flow) typically for browser-less apps that don't present the provider's sign-in page to the user. (e.g. native mobile app).

Container Apps implements container app versioning by creating revisions which are immutable snapshots of a container app version. By default, they create a unique revision name with a suffix consisting of a semi-random string of alphanumeric characters.
You can set the revision suffix in the ARM template or in the CLI using `az containerapp update`.

## Managing secrets

An updated or deleted secret doesn't automatically affect existing revisions in your app. You can respond to changes by deploying a new revision or restarting an existing revision.
Container Apps doesn't support Azure Key Vault integration.

## Dapr Integration with Container Apps

Dapr is a set of incrementally adoptable features that simplify the authoring of distributed microservice-based applications.

Dapr core concepts:

- Dapr is enabled at the container app level by configuring a set of Dapr arguments. These values apply to all revisions of a given container app when running in multiple revisions mode.
- The fully managed Dapr APIs are exposed to each container app through a Dapr sidecar. The Dapr APIs can be invoked from your container app via HTTP or gRPC. The Dapr sidecar runs on HTTP port 3500 and gRPC port 50001.
- Dapr uses a modular design where functionality is delivered as a component. Dapr components can be shared across multiple container apps. The Dapr app identifiers provided in the scopes array dictate which dapr-enabled container apps load a given component at runtime.
