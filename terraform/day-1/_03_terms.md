# Provisioning / Deployment / Orchestration

## Provisioning

To prepare a server with systems, data and software and make it ready for operation using configuration management tools.

> when you launch a cloud service and configure it, you are "provisioning"

## Deployment

The act of delivering a version of an application to run a provisioned server.

Tools used for this purpose could be: Jenkins, Circle CI, Github Actions.

## Orchestration

The act of coordinating multiple systems or services.

A common term when working with microservices, containers, kubernetes.


# Configuration drift

Is when infra has an *unexpected configuration change* due to:

- manually modifying configuration options
- malicious actors
- APKs, CLIs, SDKs side effects

## How to detect?

Cloud services provide solutions for that: AWS Config, Azure Policies, GCP Security Health Analytics

## How to correct?

- A compliance tool
- Terraform `refresh`
- Manually correcting the misconfiguration (not recommended)
- Tearing down and setting up infra again


# Mutable / Immutable infra

## Mutable (ok-ish)

develop > deploy > configure (tool/cloud init)

A VM is deployed and then a configuration tool is used to configure the state of the server.

## Immutable (recommended)

develop > configure (tool/cloud init) > deploy (packer)

A VM is launched and provisioned and finally turned into a virtual image stored in an image repository.

That image is referenced to deploy VM instances.

### Immutable infra guarantee

> Terraform encourages immutable infra architecture

So you don't have to deal with:

- Cloud resource failure: EC2 failing?
- Application failure: post-install script fails due to change in package?
- Time to deploy: urgency?
- Worst case scenario: accidental deletion // need to change regions // etc
- No guarantee 1-to-1: E.g cloud-init can't guarantee it will run the post-install script 1-to-1 with your VMs


# GitOps

Is when you take an IaC and use a git repo to introduce a process to review and accept changes to the infrastructure code. 

Once accepted, it automatically triggers a deployment.
