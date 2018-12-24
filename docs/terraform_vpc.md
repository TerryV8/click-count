# Choose AWS as a provider and create a VPC


Edit aws-vpc.tf file, which does following tasks:
- # Set up the provider for AWS. 
Actually, it defines the configuration for the choosen cloud provider. Terraform has support for various other providers aw well like Google Compute Cloud, DigitalOcean, and Heroku. 
- Create a VPC. 
- Set the options for internal VPC DNS resolution


```console
provider "aws" {
  access_key = "${var.access_key}"
  secret_key = ${var.secret_key}"
  region = "${var.region}"
}

resource "aws_vpc" "default" {
  cidr_block = "${var.vpc_cidr}"
  enable_dns_hostnames = true
  enable_dns_support = true
  tags {
    Name = "clickcount-vpc"
  }
}

```

The provider block defines the configuration for the choosen cloud provider, which is aws in our case. Terraform has support for various other providers like Google Compute Cloud, DigitalOcean, and Heroku. 

The resource block defines the resource being created. The above example creates a VPC with a CIDR block of 10.0.0.0/16 and attaches a Name tag clickcount-vpc. 

Parameters accept string values that can be interpreted when wrapped with ${}. Specifying ${var.access_key} will read the value provided for the variable access_key.

Launch:
```console
terraform plan
terraform apply
```
will create the VPC 
by prompting you to to input AWS access and secret keys. 


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
  cidr_block:                "" => "10.0.0.0/16"
  default_network_acl_id:    "" => "<computed>"
  default_security_group_id: "" => "<computed>"
  enable_dns_hostnames:      "" => "1"
  enable_dns_support:        "" => "0"
  main_route_table_id:       "" => "<computed>"
  tags.#:                    "" => "1"
  tags.Name:                 "" => "clickcount-vpc"
aws_vpc.default: Creation complete

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

The state of your infrastructure has been saved to the path
below. This state is required to modify and destroy your
infrastructure, so keep it safe. To inspect the complete state
use the `terraform show` command.

State path: terraform.tfstate
```

The above command will save the state of your infrastructure to the terraform.tfstate file.
This file will be updated each time you run that above command.
You can inspect the current state of your infrastructure by running:
```console
"terraform show".
```

Finally, you can verify the VPC has been created by visiting the VPC page on AWS WEB UI console.


# Recommandation for protecting access and secret keys from malicious attacks

Variables can either be entered using command arguments by specifying -var 'var=VALUE’. For example: terraform plan -var 'access_key=foo' -var 'secret_key=bar'.

However, "terraform apply" will not save your input values (access and secret keys). You'll be required to provide them for each update. 

To avoid inputting values for each update, create a terraform.tfvars variables file with your access and secret keys (replace foo and bar with your values):

Edit terraform.tfvars:
```console
access_key = "foo"
secret_key = "bar"
```
There is a best practice not to upload this file to your source control system (eg. Git). For Git users, make sure to include terraform.tfvars in the .gitignore file.

