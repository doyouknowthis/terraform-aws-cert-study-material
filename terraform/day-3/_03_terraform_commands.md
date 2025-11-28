# Terraform Commands

## terraform init

Initializes the terraform project by
- downloading plugins
- creating the .terraform directory
- creating the .terraform.lock.hcl dependency lock file

> this command is always the first command to run when working with terraform \
> if you modify or change dependencies, run the same command again so it apply the changes

- `terraform init -upgrade`: upgrades plugins to the latest version
- `terraform init -get-plugins=false`: skips downloading plugins
- `terraform init -plugin-dir=<dir>`: forces plugin installation in the given directory path
- `terraform init -lockfile=<MODE>`: set a dependency lock file mode

> dependency lock is named: terraform.lock.hcl \
> state lock is named: terraform.tfstate.lock.hcl

## terraform get

Used to download and update modules in the root module

`terraform get` is lightweight option when you don't want to run `terraform init` and only update modules.

> IN MOST CASES, you want to run `terraform init` instead

## terraform fmt 

Rewrites configuration files to a standard format and style. 

Applies a set of language style conventions, along with minor readability adjustments.

- indentation
- syntax errors

> `terraform fmt --diff` shows the changes that will be made 

## terraform validate

Validates the syntax of all Terraform configuration files in a directory.

Runs checks to verify configuration files are syntactically valid, including correctness of variable and resource references.

> `terraform plan` and `terraform apply` will run this command automatically.

**IMPORTANT: it won't check the correctness of a variable type. Checks run locally, so, if a variable expects a string instead of a number, it will pass the validation**

## terraform console

An interactive shell for evaluating expressions

## terraform plan

The commands create the execution plan and show it.

What it does is:
- check the current state and make sure the state is up-to-date
- compare the current state with the desired state and noting differences
- show the changes that will be made

> It will simply show a list of changes, but it won't make any. \
> `terraform plan` file is a binary file. \
> You can use `terraform plan -out=plan.out` to save the plan to a file and later use it for `terraform apply` command

## terraform apply

Applies the changes required to reach the desired state of the configuration.

> simplest way is run `terraform plan` and then `terraform apply` followed by a prompt. type 'yes' and does it.

- Automatic plan mode: `terraform apply -auto-approve`
- Saved plan mode: `terraform plan -out=plan.out` and then `terraform apply plan.out`. **It won't prompt for approval.**

> `terraform show plan.out` shows the changes that will be made 
