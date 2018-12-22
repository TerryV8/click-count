# Variables for your Infrastructure

Configurations can be defined in any file with a .tf extension using terraform syntax or as json files. It is a general practice to start with a variables.tf file that defines all of the variables that can be easily changed to tune your infrastructure.

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
  default = "eu-west-3a"
}

variable "vpc_cidr" {
  description = "CIDR for VPC"
  default = "10.0.0.0/16"
}

variable "public_subnet_cidr" {
  description = "CIDR for public subnet"
  default = "10.0.0.0/24"
}

variable "private_subnet_cidr" {
  description = "CIDR for private network"
  default = "10.0.1.0/24"
}
```

The variable block defines a single input variable that your configuration will require to provision your infrastructure. The description parameter is used to describe what the variable is for and the default parameter gives it a default value. Our example requires that you provide access_key and secret_key variables and optionally provide region, region will otherwise default to us-west-1 when not provided.

