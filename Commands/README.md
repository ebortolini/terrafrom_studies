# Terraform Commands

## terraform validate
Check if the configuration file is valid.

```shell
terraform validate
```

## terraform fmt

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

## terraform show

Shows the current state of the infrastructure

```terraform
terraform show
```

if we want to see in json format:

```terraform
terraform show -json
```

## terraform providers

Show the providers

```
terraform providers
```

## terraform output

prints all output variables from a directory

```
terraform output
```

## terraform refresh

Refresh the current state accordingly with the actual infrasturcture.
Can be used when we did some manual changes on the infrastructure.

I won't midify ny config file but the state file.

```terraform
terraform apply -refresh-only
```

## terraform graph

Show dependencies

```terraform
terraform graph
```

The output is hard to understand. We can use other tools like `graphviz`

```terraform
terraform graph | dot -Tsvg > graph.svg
```

