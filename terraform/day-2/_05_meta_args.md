# Meta Arguments

can be used with **any resource** to change the behavior of the resource.

> depends_on: explicit dependency \
> count: number of resources to create (for loop) \
> for_each: iterate over a map \
> provider: change the provider \
> lifecycle: lifecycle hooks \
> provisioner/connection: extra actions after a resource is created


## depends_on

The order of which resources are created is important when there is a dependency between resources. Usually not required to add explicit dependencies. 

Except when you know (after resources were created) that something is wrong.

```hcl
resource "aws_iam_policy" "example" {
  name        = "example"
  path        = "/"
  description = "My example policy"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  depends_on    = [aws_iam_policy.example]
}
```

## count

when managing a pool of objects, you can use count to create multiple resources.

> count only accepts whole numbers AND must be know before hand.

```hcl
resource "aws_instance" "example" {
    count = 4 # amount of instances you want to create. it starts with 0
    tags = [
        Name = "example-${count.index}" # index is the number of the instance
    ]
}
```

## for_each

Similar to count, but you can use a map to iterate over.

> could be a map or an array (remember to use `toset` to the list)

```hcl
resource "aws_instance" "example" {
    for_each = {
        group_a = "us-east-1a"
        group_b = "us-east-1b"
        group_c = "us-east-1c"
    }
    name = "example-${each.key}"
    region = each.value
    tags = {
        Name = "example-${each.key}" # current key
        Region = each.value # current value
    }
}
```

# Resource behavior

- create: creates a resource
- destroy: a resource exists in the state and it's going to be destroyed
- update in-place: a resource exists in the state and one or more arguments are going to be updated
- destroy and then create: an argument has changed, and due to api limitations, is going to be re-created

## lifecycle

- create_before_destroy: **creates a new resource** before destroying it
- prevent_destroy: prevents the resource from being destroyed
- ignore_changes: ignores changes to a specific argument. eg Tags, etc

```hcl
resource "aws_instance" "example" {
    lifecycle {
        create_before_destroy = true
    }
}
```


# Resource providers and aliases

If you need to overwrite the default provider, you can use an `alias`.

```hcl
provider "aws" {
    alias = "my-alias"
    region = "us-east-1"
}

resource "aws_instance" "example" {
    provider = aws.my-alias
}
``
