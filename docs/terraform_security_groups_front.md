# Security group for the front

For the front, we are going to build our infrastructure based on this assumption:
- 1 security group allowing incoming HTTP from the Load Balancer to the Web Apps.
- The public subnet is accessible and all traffic (0.0.0.0/0) is routed directly to the Internet gateway
- 1 security group allowing incoming SSH and ICMP from a specific IP to all instances


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
    Name = clickcount-sg_LB_to_WebApps
  }
}
```
