# Backends

Each Terraform configuration can specify a backend, it defines where and how operations are defined

## Standard backends

- Only store state
- Does not perform Terraform operations. eg Terraform apply
- Third-party backends are standard backends. eg. AWS S3

Some examples are:

- AWS S3 + DynamoDB (locking)
- Google Cloud Store (comes with locking)
- Postgres Database (comes with locking)
- there are many more, some have an option for locking, other don't

```hcl
backend "s3" {
    bucket = "terraform-state"
    key = "statefile"
    region = "us-east-1"
}
```

A backup of the state will be stored locally

## Enhanced backends

- Can both store state and perform terraform operations
  - local: files and data are stored in a local machine
  - remote: terraform cloud

### Local Backend

- Stores state on the local filesystem
- Locks the state using system APIs
- performs operations locally

```hcl
terraform {
}
```

By default, you are using a local backend when you don't specify one

```hcl
terraform {
    backend "local" {
        path = <relative_path>/terraform.tfstate
    }
}
```

This way you can cross-reference states
```hcl
data "terraform_remote_state" "my_external_state" {
    backend = "local"
    config = {
        path = <path_to_local_state>
    }
}
```

### Remote Backend

It uses a terraform platform, it can be either:
- terraform cloud
- terraform enterprise

With a remote backend, the terraform platform is responsible to execute operations (plan/apply/etc)

> Environment variables will be required during execution of commands so, it has to be configured beforehand. There is a section where you can set up credentials and more

Additionally, you will have to set a terraform (cloud) workspace

```hcl
terraform {
    backend "remote" {
        hostname = ""
        organization = ""
        
        workspaces {
            name = "" # either a single workspace (the exact name)
            prefix = "" # or multiple workspaces via prefix
        }
    }
}
```

## Backend initialization

### Cloud backend

Both options are correct, but it is recommended to use #2

```hcl
terraform {
    backend "remote" {
        ...
    }
}

# when using Terraform Cloud you have to use block "cloud" instead
terraform {
    cloud {
        ...
    }
}
```

## Backend initialization

`-backend-config` flag for `terraform init` command can be used for _partial configuration_

> on occasions where backend configuration is dynamic or sensitive and cannot be statically specified

```hcl
# main.tf
terraform  {
    backend "remote" {...}
}

# backend.hcl
workspaces { name = "workspace" }
hostname = ""
organization = ""

# console
terraform init -backend-config=backend.hcl
```

### terraform_remote_state 

It's a data source that can retrieve the root module output values from another Terraform configuration using the latest snapshot available

```hcl
data "terraform_remote_state" "my_state" {
    backend = "remote"
    ...
}

resource "aws_instance" "ec2" {
    subnet_id = data.terraform_remote_state.my_state.outputs.subnet_id # it has to come from "outputs", for this, your root module has to expose such output
}
```

An alternative and more recommended way is to use specific "data" sources

```hcl
data "aws_s3_bucket" "my_bucket" {
    bucket = ""
}

resource "whatever_aws_resource" "some_name" {
    bucket_id = data.aws_s3_bucket.my_bucket.id
}
```

## State locking

Terraform will lock the state for all ops that could write state

This prevents others from acquiring the lock and possibly corrupt the state

> it happens automatically on all write ops. it's not visible in the console \
> if locking takes longer, you will see a warning message

- `-lock`: flag disables locking
- `force-unlock`: command to manually unlock the state if unlocking failed
    - be aware that doing so could potentially open the door to a state with multiple writes
    - force-unlock should only be used on the situations where unlocking fails

```hcl
terrform force-unlock <unique_id> (-force) # to avoid confirmation
# terraform will output the <unique_id> if unlocking fails
```

### Protecting sensitive data

Terraform state can contain sensitive data, like passwords, keys, etc.

Local states are not secure, do not share the file with anyone

Remote states with Terraform Cloud is secure in that scenario

Be careful with Third-party backends and remote states, review the capabilities of the remote backend before commit to that solution

For example, AWS S3, you have to enable versioning and encryption and possibly create a custom trail for of events

## Terraform ignore file

You can define paths to ignore from upload with `.terraformignore` file (at the root module directory)

By default, Terraform Cloud builds an archive and uploads it to the remote backend

If no `.terraformignore` file is present, these folders are not included in the archive:
- .git/ directories
- .terraform/ directories

> `.terraformignore` is similar to `.gitignore` \
> the difference is, you can only have one `.terraformignore` file. Only the one in root module directory is used