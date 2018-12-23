# Variables for your Infrastructure

Configurations can be defined in any file with a .tf extension using terraform syntax. 
The variables.tf file defines all the variables that tune your infrastructure.

Edit variables.tf:
```console
variable "access_key" {
  description = "AWS access key"
}

vaariable "secret_key" {
  description = "AWS secret access key
}

variable "region"  {
  description = "AWS region to host your network"
  default = "eu-west-3"
}

variable "availability_zone" {
  description = "AWS availability zone in zone a"
  default = ["eu-west-3a","eu-west-3b"]
}

variable "vpc_cidr" {
  description = "CIDR for VPC"
  default = "10.123.0.0/16"
}

variable "public_subnet_cidr" {
  description = "CIDR for public subnet"
  default = ["10.123.0.0/24","10.123.1.0/24"]
}

variable "private_subnet_cidr" {
  description = "CIDR for private subnet"
  default = "10.123.10.0/24"
}
```


