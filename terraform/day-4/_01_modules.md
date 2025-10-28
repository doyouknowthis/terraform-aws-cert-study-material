# Modules

## How to find modules?

Terraform Registry it's the best place

## Use modules

`terraform init` will download plugins in both cases

- Public modules
```hcl
module "consul" {
    source = "hashicorp/consul/aws" # namespace/name/provider
    version = "0.1.0"
}
```

- Private modules

For private module you might need to configure Terraform Cloud via `terraform login`
```hcl
module "vpc" {
    source = "app.terraform.io.example_corp/vpc/aws" # hostname/namespace/name/provider
    version = "0.1.0"
}
```

## Publishing modules

Published modules support:

- versioning
- automatically generated docs
- allow browsing history
- show examples
- README

Repo names must match the format: terraform-<provider>-<name>

Public modules are managed via a public Git repo or Github. Once registered you can continue publishing version by simply creating tags

## Verified Modules

Modules are reviewed by Hashicorp and actively maintained by contributors.

A badge (blue hexagon checkmark) is added to a module.

This doesn't mean not verified modules are bad quality or risky.

It could be also that it wasn't created by a Hashicorp's partner


## Standard Module Structure

The primary entrypoint is the root module

Required files are:

- main.tf
- variables.tf
- outputs.tf
- README
- LICENSE

Nested modules are optional and must be contained in `modules/` directory

- if it has a README it means it can be used by external users
- better to avoid relative paths in those cases
