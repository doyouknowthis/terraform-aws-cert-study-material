# Providers

Terraform plugins. Could be:
- cloud providers
- SaaS
- other apis, eg: kubernetes, postgres, etc.

There are three tiers:
- official: published by the company that owns the service
- verified: up-to-date, actively maintained and compatible with Terraform and provider. 
- community: published by the community, but not guaranteed to be maintained or stay up-to-date.

> terraform init command downloads the provider so it can be used in the configuration.


## Registry

A website portal where plugins are published, and documentation is available.

## Terraform Cloud - Private Registry

Avaiable for Terraform Enterprise. Terraform Cloud allows you to host your own private registry and publish private modules.


# Providers/Command

CLI to list available providers.

> terraform providers

### Provider/Configuration

```terrform
provider "aws" {
    alias = "aws-us-east-1"
    region = "us-east-1"
}

resource "aws_instance" "web" {
    provider = aws.aws-us-east-1
}

module "vpc" {
    source = "terraform-aws-modules/vpc/aws"
    providers = {
        aws = aws.aws-us-east-1
    }
}

terraform {
    required_providers {
        my_aws = {
            source = "hashicorp/aws"
            version = "~> 3.0"
        }
    }
}
```


# Modules

Is a group of resources that can be reused.

- Enforces a consistent way of creating resources.
- Allows for sharing and reusing resources.
- Allows for versioning.
- Allows for testing.
- Allows for collaboration.
