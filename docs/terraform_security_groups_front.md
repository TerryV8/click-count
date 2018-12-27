# Security group for the front

For the front, we are going to build our infrastructure based on this assumption:
- 1 security group allowing incoming HTTP from the Load Balancer to the Web Apps.


Edit security_groups_front.tf:
```console
resource aws_security_group "sg_LB_to_WebApps" {
  name = "sg_LB_to_WebApps"
  description = "allowing incoming HTTP from the Load Balancer to the Web Apps"
  vpc_id = "${aws_vpc.default.id}"
  ingress {
    from_port = 80
    to_port = 80
    protocol = "tcp"
    security_groups = ["${aws_security_group.sg_internet_to_LB.id}"]
  
  }
  tags {
    Name = "clickcount_sg_LB_to_WebApps"
  }
}
```

Also, we will make another assumption:
- 1 security group allowing incoming SSH and ICMP from a specific IP to all instances

Append security_groups_front.tf:
```console
resource aws_security_group "sg_ssh_and_ping" {
  name = "sg_ssh_and_ping"
  description = "allowing incoming SSH and ICMP from a specific IP to all instances"
  vpc_id = "${aws_vpc.default.id}"
  
  ingress {
    from_port = 22
    to_port = 22
    protocol = "ssh"
    cidr_blocks = ["176.151.42.54/32"]
  }
  
  ingress {
    from_port = -1
    to_port = -1
    protocol = "icmp"
    cidr_blocks = ["176.151.42.54/32"]
  }
  
  tags {
    Name = "clickcount_sg_ssh_and_ping"
  }
}
```



- The public subnet is accessible and all traffic (0.0.0.0/0) is routed directly to the Internet gateway
