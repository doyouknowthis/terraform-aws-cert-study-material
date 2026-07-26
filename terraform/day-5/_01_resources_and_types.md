# Resources

Represent an infra object, like VM, databases, storage, etc.

```hcl
resource "aws_instance" "example" { # aws_instance is a resource
    ...
}
```

> some resource types provide special timeouts nested block that allow you to set different timeouts before being considered to have failed

```hcl
resource "aws_db_instance" "example" {
    ...
    timeouts {
        create = "10m"
        delete = "10m"
    }
}
```

# Complex types

It's a type that groups multiple types together.

Most of them are represented by type constructors, and several of them have a shorthand keyword version

There are two types of complex types:
- collection types (for grouping similar values)
  - List, Map, Set
- structural types (for grouping potentially dissimilar values)
  - Tuple and Object

## Collection types

- List: It's like any array, you access an element by its index

```hcl
variable "planets" {
    type = list(string)
    default = ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune"]
}
---
${var.planets[0]}
```

- Map: It's like a dictionary, you access an element by its key

```hcl
variable "planets" {
    type = map(string)
    default = {
        Mercury = "Mercury"
        Venus = "Venus"
        Earth = "Earth"
    }
}
---
${var.planets["Mercury"]}
```

- Set: It's similar to a list, but has no secondary index and doesn't preserve order. items are casted to the match the first element type

```bash
toset(["a","b",3])

# => ["a", "b", "3"]
```

## Structural types

Structural types require a schema as an argument to specify which types are allowed.

```hcl
variable "planets" {
    type = object({
        name = string
        radius = number
    })
    default = {
        name = "Earth"
        radius = 6371
    }
}
```

- Object: It's a map with more strict keying rules
- Tuple: It's a list with more strict element rules

```hcl
tuple([string, number, bool])

# ["a", 1, true]
```
