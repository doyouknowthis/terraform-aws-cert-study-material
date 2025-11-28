# Provisioners

Provisioners install stfw, edit files and provision machines created with Terraform.

There are 2 options: cloud-init and packer (it seems like cloud-init is preferred)

> provisioners should be used as a last resort. there are better alternatives.

**The main reason is that is out of the scope of Terraform (infra). Scripts are not reflected in a plan.**


# Commands

## Local-exec

Allows you to execute a (local) command _after_ a resource is provisioned.

> the machine where Terraform is running is where the command will be executed.

Parameters
- command: required. The command to execute.
- working_dir: optional. Where the command will be executed.
- interpreter: optional. The interpreter to use. eg: bash, aws cli, powershell, etc
- environment: optional. Environment variables to set.

```terraform
resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "echo Hello World"
  }
}
```


## Remote-exec

Allows you to execute a command **on a target server** _after_ a resource is provisioned.

Used for simple tasks. It's better to use cloud-init or other tools.

> for more complex task, better use cloud-init

Parameters
- inline: list of command strings
- script(s): path to a script to execute in the target server

```terraform
resource "null_resource" "example" {
  provisioner "remote-exec" {
    inline = [
      "echo Hello World",
```


## File

Allows you to upload a file to a newly created resource.

> you may need a connection block within the file provisioner for authentication

Parameters
- source: path to the file to upload
- destination: path to the file on the resource
- content: a file or directory

```terraform
resource "null_resource" "example" {
  provisioner "file" {
    source = "index.html"
    destination = "/var/www/html/index.html"
  }
}
```


## Connection block

Tells a provisioner how to connect to the target server.

> there are multiple 'types' of connections. eg: ssh, winrm, etc

```terraform
resource "null_resource" "example" {
  provisioner "file" {
    connection {
      type = "ssh"
      user = "ubuntu"
      private_key = file("~/.ssh/id_rsa")
      host = aws_instance.example.public_ip
    }
    source = "index.html"
    destination = "/var/www/html/index.html"
  }
```


## Null resource

Placeholder for a resource that has no association to a provider resources.

Parameters:
- triggers: a map of key/value pairs that will trigger the resource to be recreated.


## Terraform Data

Similar to `null_resource`. It doesn't require the configuration of a provider.

> practically interchangeable with null_resource. Might be recommended to use terraform_data instead of null_resource.
