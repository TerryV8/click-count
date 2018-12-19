# The first terraform resource: VPC

Edit aws-vpc.tf:
```console
provider "aws" {
  access_key = "${var.access_key}"
  secret_key = ${var.secret_key}"
  region = "${var.region}"
}

resource "aws_vpc" "default" {
  cidr_block = "${var.vpc_cidr}"
  enable_dns_hostnames = true
  tags {
    Name = "airpair-example"
  }
}

```

The provider block defines the configuration for the cloud providers, which is aws in our case. Terraform has support for various other providers like Google Compute Cloud, DigitalOcean, and Heroku. 

The resource block defines the resource being created. The above example creates a VPC with a CIDR block of 10.128.0.0/16 and attaches a Name tag airpair-example. 

Parameters accept string values that can be interpolated when wrapped with ${}. In the aws provider block specifying ${var.access_key} for access key will read the value from the user provided for variable access_key.

Launch:
```console
terraform apply
```
will create the VPC by prompting you to to input AWS access and secret keys. 

For default values, hitting <return> will assign default values, defined in the variables.tf file.

OUTPUT:
```console
var.access_key
  AWS access key

  Enter a value: foo

...

var.secret_key
  AWS secret access key

  Enter a value: bar

...

aws_vpc.default: Creating...
  cidr_block:                "" => "10.128.0.0/16"
  default_network_acl_id:    "" => "<computed>"
  default_security_group_id: "" => "<computed>"
  enable_dns_hostnames:      "" => "1"
  enable_dns_support:        "" => "0"
  main_route_table_id:       "" => "<computed>"
  tags.#:                    "" => "1"
  tags.Name:                 "" => "airpair-example"
aws_vpc.default: Creation complete

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

The state of your infrastructure has been saved to the path
below. This state is required to modify and destroy your
infrastructure, so keep it safe. To inspect the complete state
use the `terraform show` command.

State path: terraform.tfstate
```

The above command will save the state of your infrastructure to the terraform.tfstate file. This file will be updated each time you run terraform apply. You can inspect the current state of your infrastructure by running terraform show.

You can verify the VPC has been created by visiting the VPC page on AWS console.

Variables can also be entered using command arguments by specifying -var 'var=VALUE’. For example: terraform plan -var 'access_key=foo' -var 'secret_key=bar'.

However, terraform apply will not save your input values (access and secret keys). You'll be required to provide them for each update. To avoid inputting values for each update, create a terraform.tfvars variables file with your access and secret keys (replace foo and bar with your values):

Edit terraform.tfvars:
```console
access_key = "foo"
secret_key = "bar"
```
It is a best practice not to upload this file to your source control system. For git users, make sure to include terraform.tfvars in the .gitignore file.

