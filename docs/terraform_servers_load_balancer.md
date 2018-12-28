# 1 Load Balancer

The ELB in the public subnet allows us to manage distributing incoming web traffic across the group of the web application servers that it is targeted.

We will use our security group allowing incoming HTTP from Internet to the Load Balancer, which then forward HTTP traffic on the port 80.


Edit servers_load_balancer.tf:
```console
resource "aws_elb" "servers_LB" {
  name = "servers_LB"
  listener {
    lb_port = 80
    lb_protocol = "http"
    instance_port = 80
    instance_protocol = "http"  
  }
  subnets = ["${aws_subnet.public-a.id}", "${aws_subnet.public-b.id}", "${aws_subnet.public-c.id}"]
  

}
```
  
