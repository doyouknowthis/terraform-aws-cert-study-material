# Vault

It's a tool to securely access secrets from multiple secrets and resources

Vault provides 
- a unified interface to any secret (AWS Secrets Manager, Hashicorp Vault, Azure Key Vault, etc.)
- access control JIT (Just In Time) and JEP (Just Enough Privileges)
- recording a detailed audit log

> Vault is deployed into a virtual server 


## Terraform and Vault

Imagine you work with AWS resources, you will need access to AWS credentials.

> AWS credentials are long-lived and, if stored in a local machine, they could be at risk

If we could
- provide credentials just in time
- and expire them after a short time

we can reduce the attack surface area of the local machine.

**Vault can inject short-lived credentials at the time of `terraform apply`**

### Vault injection via data source

- A vault server is provisioned
- A vault engine is configured. e.g. AWS Secrets
- Vault will create a machine user for AWS
- Vault will generate short-lived credentials for the machine user
- Vault will manage and apply the AWS policy

```hcl
data "vault_aws_access_credentials" "creds" {
  backend = "aws"
  role    = "my-role"
}

provider "aws" {
  access_key = data.vault_aws_access_credentials.creds.access_key
  secret_key = data.vault_aws_access_credentials.creds.secret_key
}
```