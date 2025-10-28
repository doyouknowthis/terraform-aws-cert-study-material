# Workspaces

Allows you to manage multiple environments or alternate state files

Two variants

- CLI workspaces: manage state files either locally or remotely
- Terraform Cloud workspaces: completely different thing, it's more like projects or folder

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