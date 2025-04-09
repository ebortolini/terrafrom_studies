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

- resource is the block name
- local_file resource type
  - local = provider
  - file=resource
- pet resource name
- filename/content are arguments

### Simple Terraform workflow
- **Step 1:** Write the config file
- **Step 2:** Run init command
- **Step 3:** Review the execution plan
- **Step 4:** Apply the changes

## Update and Destroy
- Update
  ```
  terrafrom apply
  ```
- Destroy
  ```
  terrafrom destroy
  ```