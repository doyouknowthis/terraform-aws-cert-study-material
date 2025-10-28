# Variables

Parameters used by configuration

```terraform
variable "name" {
  type = string
  default = "my-instance"
  description = "Name of the instance"
  sensitive = true
  validation {
    condition = length(var.name) > 3
    error_message = "The name value must be at least 3 characters long."
  }
}
```

## Loading variables

Variable definition files can be located in:
- `TF_VAR_<name>=value` CLI
- terraform.tfvars (default file). It will be autoloaded if at root
- dev.tfvars. Not autoloaded
- terraform.tfvars.json
- prod.auto.tfvars. Files ending in .auto.tfvars will be autoloaded. It's a convenient way to separate variables
- `-var-file myfile.tfvars`. CLI
- `-var ec2_name=my-instance`. CLI

> variables could be overwritten depending on how you define variables. (see list, order goes from top to bottom)  

---

# Outputs

Values computed after `terraform apply`. It allows you to 
- obtain data regarding a resource
- output a file of values for programmatic use
- cross-reference stacks via outputs in state files via `terraform_remote_state`

```terraform
output "instance_id" {
  value = aws_instance.example.id
  description = "The ID of the EC2 instance"
  sensitive = true # still visible in state file
}
```

> `terraform output` will show all outputs. \
> `terraform output -json` will show all outputs in json format. \
> `terraform output my_output` will show a specific output.


# Local values

A block that allows to assign names to an expression. (so it can be used multiple times withing a module of terraform configuration)

```terraform
locals {
  my_name = "my-instance"
}

locals {
  my_second_name = local.my_name # reference
}
```

> it's best practice not to rely on local values too much

# Data sources

Allows Terraform to retrieve data from external sources.

```terraform
data "aws_ami" "ubuntu" {
  most_recent = true
  owners = ["099720109477"]
  filter {
    name = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}
```

# How to reference a named value

> resources = aws_instance.example.id \
> variables = var.name \
> data = data.aws_ami.ubuntu.id \
> locals = local.my_name \
> modules = module.example.instance_id \
> outputs = output.instance_id \
> **filesystem and workspace** \
> path.module = path.module \
> path.root = path.root \
> path.cwd = path.cwd. current working directory \
> workspace = terraform.workspace \
> **block-local values** \
> count = count.index. when using count meta argument \
> for_each = for_each.key. when using for_each meta argument \
> self.<attribute> self reference within the block