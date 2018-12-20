# Security groups

We will be creating 3 security groups:

- default: default security group that allow inbound and outbound traffic from all instances in the VPC
- nat: security group for NAT instances that allow SSH traffic from the internet
- web: security group that allows web traffic from the internet

Edit security-groups.tf:
```console
/* Default security group */
resource "aws_security_group" "default" {
  name = "default-airpair-example"
  description = "Default security group that allows inbound and outbound traffic from all instances in the VPC"
  vpc_id = "${aws_vpc.default.id}"
  
  ingress  {
    from_port = "0"
    to_port = "0"
    protocole = -1
    self = true  
  }
  
  egress  {
    from_port = "0"
    to_port = "0"
    protocole = -1
    self = true  
  }
}


/* Security group for the nat server */
resource "aws_security_group" "nat"  {
  name = "nat-airpair-example"
  description = "Security group for nat instances that allows SSH and VPN traffic from internet. Also allows outbound HTTP[S]"
  vpc_id = ${aws_vpc.default.id}
  
  ingress {
    from_port = 22
    to_port = 22
    protocole = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port = 1194
    to_port = 1194
    protocol = "udp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port = 80
    to_port = 80
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port = 443
    to_port = 443
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags {
    Name = "nat-airpair-example"
  }
  
}

/* Security group for the web */
resource "aws_security_group" "web" {
  name = "web-airpair-example"
  description = "Security group for web that allows web traffic from internet"
  vpc_id = ${aws_vpc.default.id}
  
  ingress {
    from_port = 80
    to_port = 80
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  ingress {
    from_port = 443
    to_port = 443
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  tags {
    Name = "web-airpair-example"
  }
}
```

Run terraform plan to review your changes and then run terraform apply. You should see an output like this:
```console
Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
```
