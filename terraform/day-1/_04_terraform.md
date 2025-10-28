# Terraform

Open source and clou agnostic IaC tool. Files are written using HCL (hashicorp configuration language)

It uses declarative configuration files


# Terraform Cloud

It's a SaaS, offers:

- remote state storage
- version control integrations
- collaboration


# Terraform Lifecycle

- code: you write the resources you want to provision
- init: initialize the project, pull providers and modules
- plan: speculate what will happen
- validate: ensure the code is valid
- apply: execute the plan. provisioning the infra
- (destroy): destroy the infra


# Change automation

## Change Management

A standard approach to apply changes and resolve conflicts.

This comes when a change is made to the IaC code and a resource is modified.

## Change Automation

It's a way to deal with change requests automatically.

Terraform uses *execution plans* and *resource graphs* for changesets

> changesets: a collection of commits made by an person that represents changes in a versioning repository.


# Execution plans

It's a manual review that explains what is going to change (add, update or destroy) before you accept it.

> you can visualize an execution plan by using `terraform graph` together with `GraphViz`.
> an example command looks like this: `terraform graph | dot -Tpng > graph.png`

Terraform builds a dependency graph.

> a dependency graph is a tree map represented by nodes and associations


# Terraform Core and Terraform Plugins

Core uses RPC to communicate with plugins. Written in Go.

Plugins are the exposed configuration for a provisioner or specific service


# Provisioners

Provisioners install stfw, edit files and provision machines created with Terraform.

There are 2 options: cloud-init and packer (it seems like cloud-init is preferred)

> provisioners should be used as a last resort. there are better alternatives.