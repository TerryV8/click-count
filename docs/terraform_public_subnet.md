# Adding the Public Subnet

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

// Public subnet 
resource "aws_subnet" "public" {
  vpc_id = "${aws_vpc_default.id}"
  cidr_block = "${var.public_subnet_cidr}"
  availability_zone = "us-west-1a"
  map_public_ip_on_launch  = true
  depends_on  = ["aws_internet_gateway_default"]
  tags  {
    Name = "public"
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

