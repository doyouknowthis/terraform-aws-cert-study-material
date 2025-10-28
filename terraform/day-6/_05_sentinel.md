# Sentinel

Embedded policy as a code framework integrated within Terraform platform

## What is policy as code?

When you write code to automate regulatory or governance policies

- Benefits
  - sandboxing: create guardrails to avoid dangerous actions or remove the need of manual verification
  - codification: policies are well documented and exactly represent what is enforced
  - version control:  
  - testing:
  - automation:

## Features

- Embedded: enable policy enforcement to actively reject violation behavior instead of passively detecting
- Fine-grained condition-based policies
- Multiple enforcement levels: advisory, soft and hard mandatory levels allow policy writers to warn on or reject behavior
- External information
- Multi-cloud compatible: ensure infra changes are within business and regulatory policy across multiple providers

> Sentinel is a paid service


## Examples

Sentinel policy that restricts the AZs for EC2 instances on AWS

```sentinel
import "tfplan-functions" as plan

allowed_zones = ["us-east-1a", "us-east-1b", "us-east-1c"]

allEC2Instances = plan.find_resources("aws_instance")

violatingEC2Instances = plan.filter_attribute_not_in_list(allEC2Instances, "availability_zone", allowed_zones, true) # true = warnings wil be printed for all violations

main = rule {
    length(violatingEC2Instances["messages"]) is 0
}
```

## Sentinel with Terraform

It can be integrated with Terraform Cloud as a part of your IaC pipeline

> In terraform cloud you can create a policy set and apply these to a Terraform cloud workspace


