# Expressions

Are used to refer to or compute values within the configuration

## Types and values

The result of an expression is a value of a specific type.

- primitives
    - string
    - number
    - boolean
- no type
    - null: not really a value, it's going to set whatever the default value is
- complex/structural/collection
    - list (tuple)
    - map

## strings

use double quotes. it can interpret escape sequences.

- \n: newline
- \r: carriage return
- \t: tab
- \": literal double quote
- \\: literal backslash
- ...

special escape sequences:

- $${: literal ${, without beginning interpolation
- %%{: literla %{, without beginning a template directive

also supports HEREDOC style (unix style multiline strings)

```bash
<<EOT
hello
world
EOT
```

- String interpolation: Use ${} to interpolate a value
- String directive: Allows to evaluate a conditional
```
"Hello %{ if var.name == "" }World%{ else }${var.name} %{ end }"
```
- You can do the same in HEREDOC style
```
<<EOT
%{ for name in var.names }
Hello ${name}
%{ endfor }
EOT
```
- You can stripe whitespace with %{~}
```
<<EOT
%{~ for name in var.names }
Hello ${name}
%{~ endfor }
```

## operators

basic mathematical operations

## conditional

terraform supports ternary conditional

> make sure the return type is the same in both cases

`condition ? true_value : false_value`

## for expressions

allows you to iterate over a collection. it can accept a list, a map, a tuple, a set, or an object

> on a list: [for s in var.list : upper(s)] \
> you can also get the index: [for i, v in var.list : "${i} is ${v}"] \
> on a map: [for k, v in var.map : upper(k)] \
> to return a tuple use square braces: [for k, v in var.map : upper(k)] \
> for an object, ues curly braces: {for k, v in var.map : s => upper(k)}. result is {key => value, ...}

You can use if statements inside the for expression for filter/reduce operations

> [for s in var.list : upper(s) if length(s) > 3]

## splat expressions

splat expression provides an shorter expressions for **for expressions**

> is represented by the * operator, originates from the ruby language

A result of for loop can be written in another way

```
[for s in var.list : id] -> var.list[*].id
```

## dynamic blocks

allows you to dynamically create repeatable nested blocks

> "ingress" _blocks_ will be created for as many "var.ports" are available

```hcl
resource "aws_security_group" "example" {
  dynamic "ingress" {
    for_each = var.ports
 
    content {
      from_port = ingress.value
      to_port = ingress.value
      protocol = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}
```

# version constrains

Terraform uses semantic versioning

> MAJOR.MINOR.PATCH

A version constraint is a range of acceptable versions

- no operator or =: exact version
- ! =: not equal to
- \> >= < <=: compare against an specific version
- ~>: allows the rightmost version (last number) to increment

