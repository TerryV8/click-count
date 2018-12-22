Create a Private Subnet with the CIDR range 10.128.1.0/24 and configure the routing table to route all traffic via the NAT.
Edit private-subnets.tf

```console
resource "aws_subnet" "private"  {
  vpc_id="${aws_vpc.default.id}"
  cidr_block="${var.}"


  tags{
    name="private"
  }
}

