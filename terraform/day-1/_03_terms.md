# Provisioning / Deployment / Orchestration

## Provisioning:

To prepare a server with systems, data and sftw and make it ready for operation using configuration management tools.

> when you launch a cloud service and configure it, you are "provisioning"

## Deployment:

The act of delivering a version of an application to a run a provisioned server.

Tools used for this purpose could be: Jenkins, Circle CI, Github Actions

## Orchestration:

The act of coordinating multiple systems or services.

A common term when working with microservices, containers, kubernetes.


# Configuration drift

Is when infra has an *unexpected configuration change* due to:

- manually modifying configuration options
- malicious actors
- APKs, CLIs, SDKs side effects


# Mutable / Immutable infra

## Mutable

develop > deploy > configure (tool)

A VM is deployed and then a configuration tool is used to configure the state of the server.

## Immutable (recommended)

develop > configure > deploy (packer)

A VM is launched and provisioned and finally turned into a virtual image stored in an image repository.

That image is referenced to deploy VM instances.


# GitOps

Is when you take an IaC and use a git repo to introduce a process to review and accept changes to the infrastructure code. 

Once accepted, it automatically triggers a deployment.
