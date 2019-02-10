# The private subnet for the back


We are going to add some private subnet consisted of 3 back subnets of IP ranges:
- 10.128.10.0/24 
- 10.128.11.0/24 
- 10.128.12.0/24 

Each subnet will be in 1 distinct availability zone (AZ).


Let us edit private-subnet.tf:
```console
resource "aws_subnet" "private-a" {
  vpc_id = "${aws_vpc.default.id}"
  cidr_block = "${var.private_subnet_cidr[0]}"
  availability_zone = "${var.region}a"
  tags {
    Name = "clickcount-private-subnet-a"
  }
}
  
resource "aws_subnet" "private-b" {
  vpc_id = "${aws_vpc.default.id}"
  cidr_block = "${var.private_subnet_cidr[1]}"
  availability_zone = "${var.region}b"
  tags {
    Name = "clickcount-private-subnet-b"
  }
}
  
resource "aws_subnet" "private-c" {
  vpc_id = "${aws_vpc.default.id}"
  cidr_block = "${var.private_subnet_cidr[2]}"
  availability_zone = "${var.region}c"
  tags {
    Name = "clickcount-private-subnet-c"
  }
}

resource "aws_elasticache_subnet_group" "sg-elasticache-redis"
{
  name = "sg-elasticache-redis"
  subnet_ids = ["${aws_subnet.private-a.id}","${aws_subnet.private-b.id}","${aws_subnet.private-c.id}"]  
}
```

We add the aws_elasticache_subnet_group since we are going to use later the aws_elasticache_replication_group module for creating Redis database.



Running a terraform plan will generate an execution plan for you to verify before creating the actual resources. It is recommended that you always inspect the plan before running the apply command.

Resource dependencies are implicitly determined during the refresh phase (in the planning and application phases). They can also be explicitly defined using the depends_on parameter. In the above configuration, the resource aws_subnet.public depends on aws_internet_gateway.default and will only be created after aws_internet_gateway.default is successfully created.

Launch:
```console
terraform plan
```

OUTPUT:
```console
Refreshing Terraform state prior to plan...

aws_vpc.default: Refreshing state... (ID: vpc-30965455)

The Terraform execution plan has been generated and is shown below.
Resources are shown in alphabetical order for quick scanning. Green resources
will be created (or destroyed and then created if an existing resource
exists), yellow resources are being changed in-place, and red resources
will be destroyed.

Note: You didn't specify an "-out" parameter to save this plan, so when
"apply" is called, Terraform can't guarantee this is what will execute.

+ aws_internet_gateway.default
    vpc_id: "" => "vpc-30965455"

+ aws_route_table.public
    route.#:                       "" => "1"
    route.~1235774185.cidr_block:  "" => "0.0.0.0/0"
    route.~1235774185.gateway_id:  "" => "${aws_internet_gateway.default.id}"
    route.~1235774185.instance_id: "" => ""
    vpc_id:                        "" => "vpc-30965455"

+ aws_route_table_association.public
    route_table_id: "" => "${aws_route_table.public.id}"
    subnet_id:      "" => "${aws_subnet.public.id}"

+ aws_subnet.public
    availability_zone:       "" => "us-west-1a"
    cidr_block:              "" => "10.128.0.0/24"
    map_public_ip_on_launch: "" => "1"
    tags.#:                  "" => "1"
    tags.Name:               "" => "public"
    vpc_id:                  "" => "vpc-30965455"
```

The + before aws_internet_gateway.default indicates that a new resource will be created.

After reviewing your plan, run terraform apply to create your resources. You can verify that the subnet has been created by running terraform show or by visiting the AWS console.

