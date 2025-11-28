# Functions

Terraform language includes a set of built-in functions that you can call from within your expressions to transform and combine values.

- numeric functions
- string functions
- collection functions
- encoding functions
- filesystem functions
- date and time functions
- hash and crypto functions
- ip network functions
- type conversion functions

## Numeric functions

- abs: returns an absolute value of a number

`abs(-1.5) == 1.5`

- floor: rounds down to the nearest integer

`floor(1.9) == 1`

- log: returns the logarithm of a number

`log(50, 10) == 1.6989700043360187`

`log(16, 2) == 4`

- ceil: rounds up to the nearest integer

`ceil(1.1) == 2`

- min: takes two numbers and returns the smallest
- max: takes two numbers and returns the largest
- parseint: takes a string and returns an integer. accepts a second argument for the base

`parseint("100", 10) == 100`

- pow: takes a base and an exponent and returns the base to the exponent power
- signum: returns -1, 0, or 1 depending on whether the number is negative, zero, or positive


## String functions

- chomp: removes the newline character from the end of a string

`chomp("hello\n") == "hello"`

- format: formats a string using the given arguments

`format("hello %s", "world") == "hello world"`

- formatlist: formats a list of strings using the given arguments

`formatlist("hello %s", ["world", "terraform"]) == ["hello world", "hello terraform"]`

- indent: adds a given number of spaces to the beginning of all but the first line in the given multi-line string

`indent(4, "hello\nworld") == "    hello\n    world"`

- join: joins a list of strings into a single string with a given separator

`join(",", ["hello", "world"]) == "hello,world"`

- lower: converts a string to lowercase
- regex: applies a regular expression to a string and returns the matching substring

`regex("hello|world", "hello my lonely world") == {"hello", "world"}`

- regexall: applies a regular expression to a string and returns all matching substrings
- replace: replaces a substring in a string with another string
- split: splits a string into a list of strings based on a given separator
- strrev: reverses a string
- substr: returns a substring of a string
- title: converts the first letter of each word in a string to uppercase
- trim: removes leading and trailing whitespace from a string
- trimprefix: removes a given prefix from a string
- trimsuffix: removes a given suffix from a string
- trimspace: removes leading and trailing all kind of whitespaces from a string
- upper: converts a string to uppercase

## Collection functions

- alltrue: returns true if all elements of a list are true or "true". Even empty list counts as true.
- anytrue: returns true if any element of a list is true
- chunklist: splits a list into a list of lists of a given size

`chunklist(["a", "b", "c", "d", "e"], 2) == [["a", "b"], ["c", "d"], ["e"]]`

- coalesce: returns the first non-empty value from a list of values
- coalescelist: returns the first non-empty list from a list of lists
- compact: removes all falsey/empty values from a list
- concat: concatenates a list of lists into a single list
- contains: returns true if a list contains a given value
- distinct: takes only one not duplicate element from a list (removes any duplicates)
- element: returns the element at a given index from a list
- index: returns the index of an element in a list
- flatten: takes a list of lists and flattens it into a single list
- length: returns the length of a list
- keys: returns the keys of a map
- lookup: returns the value of a key in a map, or if not found the second argument

`lookup(var.my_map, "key", "default") == "value"`

- matchkeys: you have multiple elements in lists, the function is going to return a list of elements whose indexes matches the corresponding indexes from another list

`matchkeys(["a", "b", "c"], [1, 2, 3], [1, 2]) == ["a", "b"] # because 1 and 2 are the same indexes in the first list`

- merge: merges two maps into a single map
- one: returns the first element of a list, if it's empty, null, if has more than one element, throws an error
- range: returns a list of integers from a given start to an end value
- reverse: reverses a list
- setintersection: returns the element that matches in all sublists
- setproduct: returns the cartesian product of all sublists

`setproduct([1, 2], [3, 4]) == [[1, 3], [1, 4], [2, 3], [2, 4]]`

- setsubtract: returns the difference between list one and list two (elements from list one)

`setsubtract([1, 2, 3], [2, 3, 4]) == [1]` 

- setunion: returns the union of all sublists, it's a set list
- slice: returns a slice of a list
- sort: sorts a list
- sum: returns the sum of a list of numbers
- transpose: transposes a list of lists

`transpose({"a" = [1, 2], "b" = [3, 4]}) == {"1" = ["a"], "2" = ["a", "b"], "3" = ["b"], "4" = ["b"}`

- values: returns the values of a map
- zipmap: creates a map from a list of keys and a list of values

`zipmap(["a", "b", "c"], [1, 2, 3]) == {"a": 1, "b": 2, "c": 3}`

## Encoding functions

- base64encode: encodes a string into a base64 encoded string
- base64decode: decodes a base64 encoded string
- jsonencode: encodes a value into a JSON string
- jsondecode: decodes a JSON string into a value
- urlencode: encodes a string into a URL encoded string
- textencodebase64: encodes a string into a base64 encoded string
- textdecodebase64: decodes a base64 encoded string
- yamlencode: encodes a value into a YAML string
- yamldecode: decodes a YAML string into a value
- csvdecode: decodes a CSV string into a list of lists
- base64gzip: compresses a string using gzip and encodes it into a base64 encoded string
- urlencode: encodes a string into a URL encoded string

`urlencode("hello world") == "hello%20world"`

## Filesystem functions

- abspath: returns the absolute path of a given path
- dirname: returns (only) the directory name of a given path
- pathexpand: expands a path to its absolute path (replaces ~ with the home directory, if doesn't have it, does nothing)
- basename: returns (only) the file name of a given path
- file: returns the contents of a file
- fileexists: returns true if a file exists
- fileset: returns a list of files in a directory

`fileset(path.module, "*.tf") == ["main.tf", "variables.tf"]`

- filebase64: returns the contents of a file as a base64 encoded string
- templatefile: returns the contents of a file as a template

```bash
templatefile("${path.module}/template.tpl}", {port = 8080, ...})

# template.tpl
Template for port: ${port} # the result is: Template for port: 8080
```

## Date and time functions

- formatdate: formats a date into a string

`formatdate("DD.MM.YYYY", "2020-01-01") == "01.01.2020"`

- timeadd: adds a given amount of time to a given date
- timestamp: returns the UTC current timestamp

## Hash and crypto functions

- bcrypt: returns a bcrypt hash of a string. remember there is no way to decrypt the result

- base64sha256: returns the base64 encoded SHA256 hash of a string
- sha256: returns the SHA256 hash of a string
- base64sha512: returns the base64 encoded SHA512 hash of a string
- sha512: returns the SHA512 hash of a string
- uuid: returns a random UUID
- uuidv5: returns a random UUID v5
- filesha256: returns the SHA256 hash of a file
- filebase64sha256: returns the base64 encoded SHA256 hash of a file
- filebase64sha512: returns the base64 encoded SHA512 hash of a file
- filesha512: returns the SHA512 hash of a file
- filemd5: returns the MD5 hash of a file
- md5
- rsadecrypt
- sha1
- filesha1

## IP network functions

- cidrhost: calculates a full host IP address for a given host number within a given IP network address prefix

`cidrhost("10.0.0.0/24", 2) == "10.0.0.2"`

`cidrhost("10.0.0.0/24", 268) == "10.0.1.13"`

- cidrnetmask: converts an IPv4 network address prefix given in CIDR notation into a subnet mask address

`cidrnetmask("10.0.0.0/24") == "255.255.255.0"`

- cidrsubnet: calculates a subnet address within given IP network address prefix

`cidrsubnet("10.0.0.0/24", 2, 2) == "10.0.0.2/24"`

- cidrsubnets: calculates a list of subnet addresses within given IP network address prefix

`cidrsubnets("10.0.0.0/24", 2, 2) == ["10.0.0.0/24", "10.0.0.128/24"]`

## Type conversion functions

- can: evaluates an expression and returns true or false depending on whether the expression produces a result without errors

`can(local.foo.bar) == true`

- defaults: a function to specify default values for variables
- nonsentive: takes a sensitive value and returns a copy of it so it is readable

```hcl
output "non_sensitive_value" {
  value = nonsensitive(sha256(local.sensitive_value))
}
```

- sensitive: takes any value and returns a copy of it so that Terraform will treat it as sensitive

```hcl
locals {
  sensitive_value = sensitive(file("${path.module/sensitive.txt}"))
}
```

- tobool: converts a string to a boolean
- tomap: converts a list of key-value pairs to a map
- toset: converts a list to a set
- tolist: converts a string to a list
- tonumber: converts a string to a number
- tostring: converts a value to a string
- try: evaluates all of its argument expressions in turn and returns the result of the first one that does not produce an error

```hcl
locals {
  foo = try(
    [tostring(var.example)],
    tolist(var.example)
  )
}
```
