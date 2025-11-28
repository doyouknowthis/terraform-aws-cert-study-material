# Terraform workflows

Core terraform workflow has three steps:

- write
- plan
- apply

## Individual Practitioner

As an individual, you could the following steps:

- write
  - Use you editor of choice
  - Store your code in VCS
  - Run `plan` and `validate` repeatably
  - Run tests
- plan
  - commit changes to repo
  - might have a single branch
  - once commited, proceed to apply
- apply
  - will run apply after review the plan
  - once reviewed and applied you will have to wait for provisioning
  - if there are changes, commit and push to remote repo

## Team

Mostly the same as individual, except you work with more people and will require some changes:

- CICD pipelines
- Open a PR
- Review the code, get feedback and update code
- Merge changes
- **Figure out where to store the state file**
- **ACLs for team members**
- **Figure out where to store secrets**
- **Manage multiple environments**

## Team / Terraform Cloud

Mostly the same as Team, but it resolves many of the highlighted bullet points in there.
