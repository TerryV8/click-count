# 1 Load Balancer

The ELB in the public subnet allows us to manage distributing incoming web traffic across the group of the web application servers that it is targeted.

We will use our security group allowing incoming HTTPS from Internet to the Load Balancer.

Edit servers_load_balancer.tf:
resource "aws_elb" "servers_LB" {
  name = "servers_LB"
  listener {
    lb_port = 80
    lb_protocol = "http"
    instance_port = 80
    instance_protocol = "http"  
  }
  
  

}

