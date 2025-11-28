# Resource Drift

It is when your expected resources are in a different _state_ than your expected _state_.

Terraform has three ways to resolve drifts:

- `--replace` can change a resource that has become damaged or degraded (and TF cannot detect it)
- `import` command
- `-refresh-only` command

## Replace resources

Terraform Taint command is used to mark a resource for replacement for the next time you run apply.

Why would you do that? A cloud resource could be degraded or damaged and you want to return the state to a healthy status.

> terraform tain aws_instance.my_resource \
> command deprecated in 0.152

The new way to replace resources is:

```bash
terraform apply -replace=aws_instance.my_resource
```

**And, yes, it has to be done one by one.**

### Resource addressing

Only applies when a resource was created using `for_each` or `count`. You can reference it by doing this:

> aws_instance.pool_of_instances[2] >> it's going to get the third element/resource \
> the same can be done when a resource is inside a module \
> my_module.aws_instance.pool_of_instances[1]


## Terraform import

This command allows Terraform to **import** existing remote resources into Terraform.

- First: define a placeholder. eg
```hcl
resource "aws_instance" "imported_instance" {
    # it can be left blank and fill it after importing (not automatically auto-filled)
}
```
- Run the import command
```hcl
terraform import aws_instance.imported_instance <ID_ON_REMOTE_CLOUD>
```

> `import` seems to only work one at a time


## Terraform refresh

Command read the current remote settings and updates the Terraform state to match

> Previously, the command was `terraform refresh` but it was deprecated (it didn't give the opportunity to review changes and propose solutions) \
> Now, `terraform apply -refresh-only (-auto-approve)` (basically the command `terraform refresh` ran behind the scenes)

`-refresh-only` allows you to refresh and update your state file without making changes to the remote infra

Imagine a resource was deleted manually, you have two options

- `terraform apply`: Terraform will notice the missing resource and propose create it again
- `terraform apply -refresh-only`: Terraform updates the state file to match the remote configuration


# Terraform troubleshooting

Simply put:

- `terraform fmt`, `terraform validate` or `terraform version` for syntax or configuration.
- `terraform refresh(?)`, `terraform apply` or `terraform -replace-only` for state errors
- TF_LOG env var to core or provider errors

## Debugging 

TF_LOG is not the only one, debugging can be enabled separately.

- TF_LOG_CORE
- TF_LOG_PROVIDER
- TF_LOG takes both

Possible values are the usual ones plus JSON (which is TRACE in json format)

TF_LOG_PATH can be used to dump logs into a file

### Crash logs

If Terraform ever crashes, it saves a log file with debug logs from the session as well as the panic message and backtrace to `crash.log`.
