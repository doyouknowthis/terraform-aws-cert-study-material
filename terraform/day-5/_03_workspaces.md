# Workspaces

Allows you to manage multiple environments or alternate state files

Two variants

- CLI workspaces: manage state files either locally or remotely
- Terraform Cloud workspaces: **completely different thing**, it's more like projects or folders

> workspaces are similar to git branches \
> it's technically the same as renaming your state file

It used to be called environments until Terraform 0.9

By default you already using workspaces

```bash
terraform workspace list # workspaces can be deleted, except 'default'
# default
```

## Internals

- Local state

Terraform stores states in a folder called `terraform.tfstate.d`

- Remote state

Workspace files are stored directly in the configured backend

## Interpolation

You can reference the current workspace in your configuration via `terraform.workspace`

```hcl
resource "aws_instance" "example" {
    count = terraform.workspace == "prod" ? 1 : 0
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"
```

## Multiple workspaces

A terraform configuration has a backend that:

- defines how operations are executed
- where persisted data is stored. eg. Terraform state

Multiple workspaces is a feature many remote backends support.

## Commands

- terraform workspace list: Lists all available workspaces
- terraform workspace show: Shows the current workspace
- terraform workspace select <workspace>: Selects a different workspace
- terraform workspace new <workspace>: Creates a new workspace
- terraform workspace delete <workspace>: Deletes a workspace


# Workspaces / Terraform Cloud 

A workspace in Terraform Cloud is more like an folder/project with multiple functionalities

## Run triggers

Connect your workspace to one or more workspaces via 'run triggers', AKA 'source workspaces'.

You can connect workspaces up to 20 other workspaces via 'run triggers'.

> run triggers are designed for infrastructure that relies on information of infrastructure produced by other workspaces


# Summary

- Local Terraform
    - Configuration: on disk
    - Variable values: on tfvars file, env var, command line, etc
    - State: on disk or remote backend
    - Credentials and secrets: shell environments or CLI prompt

- Terraform Cloud
    - Configuration: linked to a version control repo, or uploaded via API/CLI
    - Variable values: stored in workspace (environment variables section)
    - State: stored in workspace (states section)
    - Credentials and secrets: in workspace (secrets section, you can tick a check to set a value as sensitive)
