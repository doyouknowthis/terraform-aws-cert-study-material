# Terraform Cloud

Available on `app.terraform.io`

> It's an application that helps teams use Terraform together

It has lots of features such as:

- manages state files
- history of previous runs
- history of previous states
- easy and secure variable injection
- tagging
- run triggers
- choose different Terraform versions
- global state sharing
- commenting on runs
- notifications via hooks
- org and workspace level access control
- policy as code (sentinel)
- MFA / single sign on (business tier)
- cost estimation (teams and governance tier)
- integrations

## Terms

- Organization

A collection of workspaces

- Workspace

Represents a unique environment or stack

- Teams

A collection of users, it can be assigned to multiple workspaces

- Runs

Represents a single execution of Terraform run environment

Runs can be: UI/VCS/API/CLI driven


### Workflows

When you create a workspace, you can choose a workflow.

There are three types:

- VCS
  - You can integrate a repository with an specific branch
- API
  - uses Terraform Cloud API
- CLI
  - Runs are triggered by the user running Terraform CLI (locally, on their own machine)

## Organization Level Permissions

Manage certain resources or settings across an organization.

- Policies
- Policies overwrites
- Workspaces
- VCS settings

> An organization MUST have an Owner assigned

Organization owners have special permissions:

- publish private modules
- invite users
- manage team membership
- view all secrets 
- manage organization permissions / settings / billing
- delete an organization
- manage agents

## Workspace Level Permissions

Manage certain resources or settings across a specific workspace.

General workspace permissions are related to:

- Runs
- Lock and unlock workspaces
- variables / outputs / states
- download sentinel mocks

> An Admin is a special role for a workspace

Permissions available for admins are:

- read / write workspace settings
- set or remove workspace permissions
- delete a workspace

### API tokens

Terraform Cloud supports three types of API tokens:

- Organization
  - Only owners can generate or revoke an organization token
  - Only available ONE at a time
  - Tokens are designed for creating and configuring **workspaces** and **teams**
    - Not recommended for all-purpose use
- Team
  - Allows access to the workspace a team was assigned to. Not tied to an specific user
  - Only available ONE at a time
  - Tokens are designed for performing API operations on **workspaces**
- User
  - Could be a real user or a machine
  - Flexible (it inherits permissions form the user they are assigned with)

## Private Registry

Terraform Cloud allows you to publish modules to a private registry.

Includes

- modules versioning
- searchable and filterable list
- configuration designer

> all users within the organization can view the private registry

You can user either the user or team token for authentication, but the type of token grants or restrict you certain actions

### Cost estimation

It's a feature that gets you a monthly cost report of resources

> cost estimation is available on Teams and Governance tiers and above \
> it's limited to specific cloud resources from three major cloud providers: AWS, Azure and GCP

## Workflow options

- You can choose any version of Terraform for a workspace
- You can choose to globally share the state file
- You can choose wether to auto-approve or manually approve runs


### Migrate a local state to Terraform Cloud

- Create a workspace in Terraform Cloud
- Replace you Terraform configuration to a remote backend

```hcl
terraform {
}

# to

terraform {
    backend "remote" {
        hostname = "app.terraform.io"
        organization = "my-org"
        workspaces {
            name = "my-workspace"
        }
    }
}
```

- Run `terraform init` and confirm you want to copy the state to Cloud by typing `yes`

## Run environment

Terraform Cloud executes plan and apply commands in its own run environment

> A terraform cloud run environment is a single-use Linux machine running on a x86-64 architecture

The following environment variables are available:

- TFC_RUN_ID - a unique ID
- TFC_WORKSPACE_NAME
- TFC_WORKSPACE_SLUG
- TFC_CONFIGURATION_VERSION_GIT_BRANCH
- TFC_CONFIGURATION_VERSION_GIT_COMMIT_SHA
- TFC_CONFIGURATION_VERSION_GIT_TAG

You can access them within you Terraform configuration by adding a variable

```hcl
variable "TFC_RUN_ID" {
    type = string
}
```

## Terraform Cloud Agents

It's a paid feature of Business tier to allow Terraform to communicate with isolated, private or on-premise infrastructure

# Terraform Enterprise

It's the self-hosted distribution of Terraform Platform

Enterprise offers a private instance of Terraform Platform application with benefits such as:

- no resources limits
- audit logging
- SSO

In a nutshell, you will need:

- A license
- A machine (apparently Debian), where Terraform platform will be installed
- A postgres database
- Cloud storage or disk
- TLS certificate


### Air gapped environments

Air gap or disconnected network is a network security measure employed on one or more computers to ensure that a network 
is physically isolated from the Internet.

> Industries in the public sector or large enterprises often employ air gapped networks

Hashicorp Enterprise supports an installation type for air gap environments
