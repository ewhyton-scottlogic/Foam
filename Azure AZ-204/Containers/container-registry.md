# Container Registry

The Azure Container Registry (ARC) service allows you to store images, it can be used with existing development and deployment pipelines.

Use Cases - pull images from ACR to various deployment targets:

- Scalable orchestration systems: Kubernetes, DC/OS, Docker Swarm
- Azure services: Azure Kubernetes Services (AKS), App Service, Batch, Service Fabric, etc.

Devs can push to a container registry e.g. in a CI/CD tool like Azure pipelines or Jenkins.
You can configure ACR Tasks to automatically rebuild application images when their base images are updated or on builds.

There are three service tiers: Basic, Standard and Premium.

## Storage Capabilities

Each tier benefits from the following:

- Encryption-at-rest (automatically encrypted/decrypted on the fly)
- Regional storage (ACR stores data in the region where the registry is created. In regions (except from Brazil South and Southeast Asia) data is stored in a paired region) - If there's a regional outage data may be unavailable, if this is a no no then enable geo-replication.
- Zone Redundancy (Premium only, users AZ's to replicate the registry to a minimum of 3 zones)
- Scalable storage (create stuff up to the registry store limit)

Tags can impact the performance of the registry, as part of your maintenance routine delete unused tags and images.

## Build and Manage Containers with Tasks

ACR Tasks can be used to automate OS and framework patching for Docker containers. It enables automated builds triggered by source code updates, updates to a containers base image or timers.

ACR Tasks supports several scenarios to build and maintain container images and other artifacts.

- Quick task (push a single container image to a container registry on-demand e.g. `docker build` or `docker push` in the cloud)
- Automatically triggered Tasks
- Multi-step Task (build and push or multi container based workflows)

### Quick Task

`az acr build` in the CLI takes a context, sends it to ACR Tasks and pushing the built image to its registry.

### Trigger task on source code update

You can trigger a container image build when code is committed, a pull request made or updated (Github or Azure DevOps) e.g. you can use command `az acr task create` by specifying a git repo and Dockerfile, when code is updated ACR will build the container image.

You can trigger tasks on base image update and schedule a task by setting up one or more timer triggers when you create or update the task.

### Multi-step Tasks

These are defined in a `YAML` file. You can define the execution of one or more containers with each step using the container as its execution environment. e.g. you could create a multi-step tasks which does the following: build the web app image, run the container, build the app test image, run the test container, if tests pass build a Helm chart, perform a help upgrade using the new Helm chart.

## Dockerfile

A Dockerfile is a script that contains a series of instructions that are used to build a Docker image. They typically include:

- Base or parent image used to create the new image
- Commands to update the base OS and install other software
- Build artifacts to include e.g. developed application
- Services to expose e.g. storage and network config
- Command to run when container is launched.

See Retroflect or Summit Explorer for examples of Dockerfiles.
