# HCL

## Understanding the sintaxe:

A HCL file constains blocks and arguments.

```hcl
<block> <parameters> {
    key1 = value1
    key2 = value2
}
```

**Block:** contains information about the infrastucture platform and a set of resources whithin that platform

**Arguments:** reoresents the configuration in key/value painr format 

Example:

```hcl
resource "local_file" "pet" {
    filename = "/root/pets.txt"
    content = "We love pets!"
}
```
