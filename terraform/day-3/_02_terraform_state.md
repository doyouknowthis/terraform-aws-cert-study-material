# Terraform State

## what is a state?

A particular condition of cloud resources at specific time.

## how Terraform preserve state?

Terraform creates a file called `terraform.tfstate` in the current directory.

> It's a json file. It has all the information about the resources, objects, etc.

## available commands

- `terraform state list`: Lists resources in the state.
- `terraform state show`: Shows a resource in the state.
- `terraform state pull`: Pull current remote state and output to stdout.
- `terraform state push`: Update remote state with local state.
- `terraform state rm`: Remove resource from the state.
- `terraform state mv`: Move resource in the state.
- `terraform state replace-provider`: Replace provider in the state.

## terraform state mv

Allows to:

- rename a resource

`terraform state mv aws_instance.dummy aws_instance.new_dummy`
- move a resource to a different module
> let's better not try to rename a resource when moving it to a different module.

`terraform state mv aws_instance.dummy module.my_module.aws_instance.dummy`
- move a module into a module

`terraform state mv module.my_module module.parent.my_other_module`

## terraform state backups

All terraform state commands _that modify the state_ will write a backup file.

> file is called terraform.tfstate.backup \
> if you want to get rid of it, just delete it MANUALLY.