# Adding the Internet gateway, Public Subnet

Let us now add:
- a public subnet with the IP range 10.128.0.0/24 
- and attach an Internet Gateway 
- and routing table. 

Edit public-subnet.tf:
```console
// Internet gateway for the public subnet
resource "aws_internet_gateway" "default"  {
  vpc_id = "${aws_vpc.default.id}"
}

// Public subnet-1
resource "aws_subnet" "public" {
  vpc_id = "${aws_vpc_default.id}"
  cidr_block = "${var.public_subnet_cidr[0]}"
  availability_zone = "${var.availability_zone[0]}
  map_public_ip_on_launch  = true
  depends_on  = ["aws_internet_gateway_default"]
  tags  {
    Name = "clickcount-public-1"
  }
}

// Public subnet-2 
resource "aws_subnet" "public" {
  vpc_id = "${aws_vpc_default.id}"
  cidr_block = "${var.public_subnet_cidr[1]}"
  availability_zone = "${var.availability_zone[1]}
  map_public_ip_on_launch  = true
  depends_on  = ["aws_internet_gateway_default"]
  tags  {
    Name = "clickcount-public-2"
  }
}


// Routing table for public subnet
resource "aws_route_table" "public"  {
  vpc_ip = "${aws_vpc.default.id}"
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = ${aws_internet_gateway.default.id}
  }
}

//Associate the routing table to public subnet
resource "aws_route_table_association" "public" {
  subnet_id = ${aws_subnet.public.id}
  route_table_id = ${aws_route_table.public.id}
}

```

Running terraform plan will generate an execution plan for you to verify before creating the actual resources. It is recommended that you always inspect the plan before running the apply command.

Resource dependencies are implicitly determined during the refresh phase (in planing and application phases). They can also be explicitly defined using the depends_on parameter. In the above configuration, the resource aws_subnet.public depends on aws_internet_gateway.default and will only be created after aws_internet_gateway.default is successfully created.

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

