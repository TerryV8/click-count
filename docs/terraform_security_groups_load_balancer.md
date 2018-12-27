# Security groups for the layer Load balancer

The ELB in the public subnet allows us to manage 
distributing incoming web traffic across the group of the web application servers that it is targeted.

1 security group allowing incoming HTTPS from Internet to the Load Balancer


Edit security_groups_load_balancer.tf:
```console
resource "aws_security_group"  "sg_internet_to_LB"  {
  description = "allowing incoming HTTPS from Internet to the Load Balancer"
  vpc_id = "${aws_vpc.default.id}"
  tags {
    Name = "clickcount-sg-internet-to-LB"
  }
  
  ingress {
    from_port = 443
    to_port = 443
    protocol = "tcp"
    cidr_blocks = [
      "0.0.0.0/0"
    ]
  
  }
}

```
