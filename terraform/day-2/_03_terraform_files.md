# Hashicorp configuration files

aka Terraform files. Contain the configuration info for providers and resources.

> files ending in `.tf` or `.tf,json`. HCL language

- Basic elements

  - Blocks: containers
  - Arguments: attributes
  - Expressions: values
  
```terraform
# block
resource "aws_instance" "example" {
    # arguments
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro" # expression
}
```

## Alternative json syntax

> files ending in `.tf.json`

```terraform
{
    "resource": {
        "aws_instance": {
            "example": {
                "ami": "ami-0c55b159cbfafe1f0",
                "instance_type": "t2.micro"
            }
        }
    }
}
```

## Settings

```terraform
terraform {
    required_version = ">= 0.14"
    required_providers {
        aws = {
            source = "hashicorp/aws"
            version = "~> 3.0"
        }
    }
    experiment = "module_variable_optional_attrs"
    # provider meta: module specific info for providers
}
```