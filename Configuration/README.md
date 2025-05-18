# Configuration

## Providers

When we do `terrafrom init` the providers plugins (aws/azure ...) are installed.
They are installed in a hidden directory called .terraformplugins.

We can go here: https://registry.terraform.io/ and search for providers.
They can be:
- Official
- Partner
- Community

## Configuration Directory
A common practice is to have the following structure on a directory:
main.tf - Main configuration file containing resource definition
variables.tf - contains variable declarations
outputs.tf - Contains outputs from resources
provider.tf - conatins provider definition

## Multiple providers
Remember, when adding a new provider, we need to run `terraform init` again

## Input Variables
An example of a variables.tf file:

```terraform
variable "filename" {
    default = "/root/pets.txt"
}
variable "content" {
    default = "We love pets!"
}
```

The on main.tf

```terraform
resource "local_file" "pet" {
    filename = var.filename
    content = var.content
}
```

The variable block can also have type and description:

```terraform
variable "filename" {
    default = "/root/pets.txt"
    type = string
    description "some description"
}
```

### Variable types:
- string
- number
- bool
- any
- list
- map
- object
- tuple

**Examples**

variables.tf

```terraform
variable "prefix" {
    default = ["Mr", "Mrs"]
    type = list
}

variable "file-content" {
    type = map
    default = {
        "statement1" = "We love pets"
        "statement2" = "We love pets 2"
    }
}

variable "bella" {
    type = object({
        name = string
        color = string
        age = number
        food = list(string)
        favorite_pet = bool
    })

    default = {
        name = "bella",
        color = "brown"
        age = 7
        food = ["fish", "chicken"]
        favorite_pet = true
    }
}

variable kitty {
    type = tuple([string, number, bool])
    default = ["cat", 7, true]
}

```

main.tf

```terraform
resource "random_pet" "pet" {
    prefix = var.prefix[0]
}

resource "local_file" "my-pet" {
    filename = "/root/pets.txt"
    content = var.file-content["statement2"]
}

```

The type `set` is same as a list but can't have duplicates

### Environment variables
we can use environment variables like:

```bash
export TF_VAR_variablename="value"
```

### variable definition files
we can create a file called: terraform.tfvars

```
filename = "/path/name.txt"
content = "Some content"
```

Names like this:
terraform.tfvars or terraform.tfvars.json or *.auto.tfvars or *.auto.tfvars.json are automatically loaded

If the anme doesn't follow this patter, we can use the flag -var-file when call terraform apply

```bash
terraform apply -var-file variables.tfvars
```

#### Precedence

In the case you are setting the same variable from different ways, this is the precedence (Low to highest priority):
1 - Env
2 - terraform.tfvars
3 - *.auto.tfvars(alphabetical order)
4 - -var or -var-file

## Resource Attributes

Let's use the output from random_pet as the content of the file that we are going to generate:

```terraform
resource "local_file" "pet" {
    filename = var.filename
    content = "My favorite pet is ${random_pet.my_pet.id}
}

resource "random_pet" "my-pet" {
    prefix = var.prefix
    separator = var.separator
    length = var.length
}
```