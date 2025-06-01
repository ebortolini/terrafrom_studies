# Introduction to terraform state

When we do a `terrafrom appy` a file called ``terraform.tfstate`` is created, that way it can track the state of the terraform and know in the next apply what changed.

For sure this file should be saved in a common plave, like S3 and not in a individual lcoal machine.

## Considerations:

 - The file conatins sensitive information
 - Never edit manually the state file 