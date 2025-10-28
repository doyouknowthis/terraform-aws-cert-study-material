# what's IaC

Infrastructure as Code

You write configuration scripts to automate creating, updating, deleting cloud infra. It's like
a blueprint for infrastructure.

IaC allows you to easily share, version or inventory your infrastructure.

## Why do we want that?

Manual configuration is prone to errors. Even though we can start creating resources via ClickOps 
right away, there are some downsides:
- it's easy to misconfigure a service
- it's hard to track changes
- it's hard to transfer knowledge to team members

## Popular IaC tools

### Explicit
WYSIWYG and also more verbose. Uses scripting languages: json, yaml, xml, etc.

- ARM templates (azure)
- Azure blueprints (azure)
- Cloud Formation (aws)
- Cloud Deployment Manager (gcp)
- Terraform (many)

### Implicit
You write what you want, and the tool does the rest. Less verbose but could end up in misconfiguration.
Uses programming languages: Javascript, Python, Ruby, etc.

- AWS cloud development kit (cdk)
- Pulumi (aws, azure, gcp, k8s)

Terraform could be considered both.