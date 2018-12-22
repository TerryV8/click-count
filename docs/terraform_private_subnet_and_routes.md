Create a Private Subnet with the CIDR range 10.128.1.0/24 and configure the routing table to route all traffic via the NAT.
Edit private-subnets.tf

```console
/* Private subnet */
resource "aws_subnet" "private"  {
  vpc_id="${aws_vpc.default.id}"
  cidr_block="${var.private_subnet_cidr}"
  availibility_zone = "${var.availability_zone}"
  map_public_ip_on_launch = false
  depends_on = ["aws_instance.nat"]
  tags{
    name="private"
  }
}

/* Routing table for private subnet */
resource "aws_route_table" "private" {
  vpc_id = "{aws_vpc.default.id}"
  route {
    cidr_block = "0.0.0.0/0"
    instance_id = ${aws_instance.nat.id1}
  }
}

/* Associate the routing table to private subnet */
resource "aws_route_table_association" "private" {
  subnet_id = ${aws_subnet.private.id}
  route_table_id = ${aws_route_table.private.id}
}

```

Notice our second time use of depends_on. In the above case, depends_on only creates the private subnet after the NAT instance is created and successfully provisioned. Without the iptables configuration, the instances in the private subnet will not be able to access the internet and will fail to download Docker containers.

Run terraform plan and terraform apply to create the resources.

