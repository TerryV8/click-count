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

//Public subnet 
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



```

