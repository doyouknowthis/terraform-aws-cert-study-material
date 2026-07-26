# Packer

It's a developer tool used to build machine that will be stored in a repository.

A build image provides:

- immutable infra
- the VMs in your fleet are all one-to-one in configuration
- faster deploys for multiple servers after each build
- earlier detection and intervention of package changes or deprecations of old tech

> The flow looks like this: \
> VCS repository receives changes and starts a CICD pipeline -> \
> CICD pipeline triggers a build image to Packer -> \
> Packer could use Ansible to provision an image, then it stores the image -> \
> Once stored, the image is referenced in a Terraform configuration -> \
> finally, Terraform creates the VM

Packer configures an image via Packer template (it uses HCL)

## Packer Template file

```hcl
variable "ami_id" {
  type = string
  default = "ami-00000000000000000"
}

locals {
  app_name = "my-app"
}

# the source says _where_ and _what_ kind of image to build
source "amazon-ebs" "my-app" {
  ami_name      = "my-server-${local.app_name}"
  instance_type = "t2.micro"
  region        = "us-east-1"
  source_ami    = var.ami_id
  ssh_username  = "ubuntu"
}
# the image will be stored in AWS EC2 images

# build allows us to configure the image via scripts
# Packer supports a wide range of provisioners: chef, ansible, puppet, shell, etc.
build {
  sources = ["source.amazon-ebs.my-app"]
  provisioner "shell" {
    script = "install_app.sh"
  }
  post-processor "manifest" {
    inline = ["echo ${local.app_name}"]
  }
}
# post provisioners run after the image is built
```

## Terraform integration

There are two steps:

- Building the image: You need to manually run or automate Packer to build an image
- Reference the image: Once an image is created you can reference it in Terraform as a Data Source

```hcl
data "aws_ami" "my-app" {
  most_recent = true
  owners      = ["self"]
  name_regex  = "my-server-my-app"
}
```