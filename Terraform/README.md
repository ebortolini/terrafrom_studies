# Working with Terraform 

## Terraform Commands

### terraform validate
Check if the configuration file is valid.

```shell
terraform validate
```

### terraform fmt

scan configuration files and format the conde ina canonical format

```shell
terraform fmt
```

For example, before running the command:

```terraform
resource "local_file" "pet" {
    filename = "/root/pets.txt"
    content = "test"
}
```

After running the command:
```terraform
resource "local_file" "pet" {
    filename    = "/root/pets.txt"
    content     = "test"
}
```

### terraform show

Shows the current state of the infrastructure

```terraform
terraform show
```

if we want to see in json format:

```terraform
terraform show -json
```

### terraform providers

Show the providers

```
terraform providers
```

### terraform output

prints all output variables from a directory

```
terraform output
```

### terraform refresh

Refresh the current state accordingly with the actual infrasturcture.
Can be used when we did some manual changes on the infrastructure.

I won't midify ny config file but the state file.

```terraform
terraform apply -refresh-only
```

### terraform graph

Show dependencies

```terraform
terraform graph
```

The output is hard to understand. We can use other tools like `graphviz`

```terraform
terraform graph | dot -Tsvg > graph.svg
```

## Mutable vs Immutable Infrastructure

Terraform works on an immutable infrastructure, so basically, when we have an update on some resource, it will be destroyed first, than a new will be created.

## Lifecycle Rules

The fault behavior is always destriy a resource than create a new one.
But this is not the best approach for all the cases, and than comes the lifecycle rules.

Example:

```terraform
resource "local_file" "pet" {
    filename = "/root/pets.txt"
    content = "test"
    file_permission = "0700"

    lifecycle {
        create_before_destroy = true
    }
}
```

we also have other lifecycle types, like ``prevent_destroy`` and ``ignore_changes``.

## Data Sources

Can use elements that were not created by terraform. For example, in the following code we are going to use a file, dog.txt in our scripts using the ``data`` identification:

```terraform
resource "local_file" "pet" {
    filename = "/root/pets.txt"
    content = "Test"
    content = data.local_file.dog.content
}

data "local_file" "dog" {
    filename = "/root/dog.txt"
}

```

## Meta Arguments

They change the way the resource bahaves:
- depends_on
- lifecycle
- count
- for_each
- provider
- provisioner

### Count

variables.tf
```
variable "filename" {
    default = [
        "/root/f1.txt",
        "/root/f2.txt",
        "/root/f3.txt"
    ]
}
```

main.tf
```terraform
resource "local_file" "pet" {
    filename = var.filename[count.index]
    count = length(var.filename)
}
```

### for_each

variables.tf
```
variable "filename" {
    type=set(string)
    default = [
        "/root/f1.txt",
        "/root/f2.txt",
        "/root/f3.txt"
    ]
}
```

main.tf
```terraform
resource "local_file" "pet" {
    filename = each.value
    for_each = var.filename
}
```

## Version Constraints

``` terraform
terrafrom {
    required_providers {
        local = {
            source = "hashicorp/local"
            version = "1.4.0"
        }
    }
}
```

