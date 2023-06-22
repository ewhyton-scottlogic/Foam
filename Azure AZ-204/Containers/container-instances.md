# Azure Container Instances (ACI)

ACI offers the fastest and simplest way to run a container in Azure, without having to manage any VMs and without having to adopt a higher-level service.

ACI is great for any scenario that can operate in isolated container, including simple applications, task automation and build jobs. It has some great benefits too:

- Fast startup
- Container access
- Hypervisior-level security
- Customer data (stores the minimum customer data required to ensure container groups are running as expected)
- Custom sizes
- Persistent storage
- Linux and Windows

For scenarios where you need full container orchestration, use Azure Kubernetes Service [AKS](https://learn.microsoft.com/en-us/azure/aks/intro-kubernetes).

## Container groups

A Container group is a collection of containers that get scheduled on the same host machine. They share a lifecycle, resources, local network and storage volumes (similar to a _pod_ in Kubernetes).

Note: multi-container groups are only supported for Linux containers, Windows only supports deployment of a single instance.

### Deployment

There are 2 ways to deploy a multi-container group: ARM template or YAML file. ARM template is recommended when you deploy additional service resources, YAML file recommended when your deployment includes only container instances.

### Networking

Container groups share an IP address and port namespace on that IP address. To enable external clients to reach a container within the group, you must expose the port on the IP address from the container. Containers within a group can reach each others via localhost on the ports that they've exposed (even if those ports aren't exposed externally on the groups IP address).

Examples using Multi-container groups:

- A container serving a web application and a container pulling the latest content from source control.
- An application container and a logging container. The logging container collects the logs and metrics output by the main application and writes them to a long-term storage.
- An application container and a monitoring container. The monitoring container periodically makes a request to the application to ensure that it's running and responding correctly, and raises an alert if it's not
- A front-end container and a back-end container. The front end serves a web application and the back end runs a service to retrieve data.

## Restart Policies

When you create a container group you can specify one of the three policy settings.

| Restart Policy | Description                                                                                                            |
| -------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `Always`       | Containers in the container group are always restarted. This is on by default.                                         |
| `Never`        | Containers are never restarted. The containers run at most once.                                                       |
| `OnFailure`    | Restarted only when the process executed in the container fails (nonzero exit code). The containers run at least once. |

When the container whose restart policy is `Never` or `OnFailure`, the container status is set to Terminated.

## Environment Variables in Container Instances

If you need to pass secrets as env variables, ACI supports secure values for both Windows and Linux containers.
Environment variables with secure values aren't visible in your container's properties, the values can be accessed only from within the container.

## Mount an Azure file share in ACI

By default, ACI instances are stateless i.e. if it crashes or stalls then all of its state is lost. To persist state beyond the lifetime of the container, you must mount a volume from an external source; you can do this with Azure Files.

Limitations

- You can only mount Azure Files shares to Linux containers
- Azure File share volume mount requires the Linux container run as _root_
- Azure File share volume mounts are limited to CIFS support.

You'd use a command like this

```bash
az container create
    --resource-group $ACI_PERS_RESOURCE_GROUP
    --name hellofiles
    --image mcr.microsoft.com/azuredocs/aci-hellofiles
    --dns-name-label aci-demo
    --ports 80
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME
    --azure-file-volume-account-key $STORAGE_KEY
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME
    --azure-file-volume-mount-path /aci/logs/
```

You can also obviously deploy using YAML. To mount multiple volumes you put the volumes in an array object in JSON `"volumes":[{...},{...}]`, then to mount the volumes you use a `volumeMounts` array.
