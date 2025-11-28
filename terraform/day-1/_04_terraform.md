# Hashicorp

Company that specializes in managed open-source tools

## Products

- Boundary
- Consul
- Nomad
- Packer
- Terrform/Terraform Cloud
- Vagrant
- Vault
- Waypoint


# Terraform

Open source and clou agnostic IaC tool. Files are written using HCL (hashicorp configuration language)

> Uses declarative configuration files


# Terraform Cloud

It's a SaaS, offers:

- remote state storage
- version control integrations
- collaboration
- offers free-plan (up to 5 members)


# Terraform Lifecycle

1. code: you write the resources you want to provision
2. init: initialize the project, pull providers and modules
3. plan: speculate what will happen
4. validate: ensure the code is valid
5. apply: execute the plan. provisioning the infra
6. (destroy): destroy the infra


# Change automation

## Change Management

A standard approach to apply changes and resolve conflicts.

This comes when a change is made to the IaC code and a resource is modified.

Change management is the procedure that comes after modifications to the configuration script.

## Change Automation

It's a way to deal with change requests automatically.

**Terraform uses *execution plans* and *resource graphs* for changesets**

> changesets: a collection of commits made by an person that represents changes in a versioning repository.

In conclusion: Change Automation allows you to know what Terraform will change and in what order avoiding many possible human errors.


# Execution plans

It's a *manual review* that explains what is going to change (add, update or destroy) before you accept it.

You can visualize an execution plan by using `terraform graph` together with `GraphViz`.

> an example command looks like this: `terraform graph | dot -Tpng > graph.png`

Terraform builds a dependency graph.

> a dependency graph is a tree map represented by nodes and associations


# Terraform Core and Terraform Plugins

Core uses RPC to communicate with plugins. Written in Go.

Plugins are the exposed configuration for a provisioner or specific service
