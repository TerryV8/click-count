# 1 instance Load Balancer

The ELB in the public subnet allows us to manage the distribution of the incoming web traffic across the group of the web application servers that it is targeted.

We will use our security group allowing incoming HTTP from Internet to the Load Balancer, which then forwards HTTP traffic on the port 80.


Edit instance_load_balancer.tf:
```console
resource "aws_elb" "instance_LB" {
  name = "instance-LB"

  listener {
    lb_port = 80
    lb_protocol = "http"
#    instance_port = 80
    instance_port = 30808
    instance_protocol = "http"
  }

  subnets = [
#    "${aws_subnet.public-a.id}",
    "${aws_subnet.public-b.id}",
    "${aws_subnet.public-c.id}"
  ]

  security_groups = ["${aws_security_group.sg_internet_to_LB.id}","${aws_security_group.allow_all.id}"]

  health_check {
    healthy_threshold = 2
    unhealthy_threshold = 5
    timeout = 5
#    target = "TCP:80"
    target = "HTTP:30808/clickCount/"
    interval = 30
  }

  instances = [
#    "${aws_instance.instance_public_a.id}",
    "${aws_instance.instance_public_b.id}",
    "${aws_instance.instance_public_c.id}",
  ]
}





resource "aws_elb" "instance_LB" {
  name = "instance-LB"

  listener {
    lb_port = 80
    lb_protocol = "http"
    instance_port = 80
    instance_protocol = "http"  
  }

  subnets = [
#    "${aws_subnet.public-a.id}",
    "${aws_subnet.public-b.id}",
    "${aws_subnet.public-c.id}"
  ]
  
  security_groups = ["${aws_security_group.sg_internet_to_LB.id}"]
  
  health_check {
    healthy_threshold = 2
    unhealthy_threshold = 5
    timeout = 5
    target = "TCP:80"
    interval = 30
  }
  
  instances = [
#    "${aws_instance.instance_public_a.id}",
    "${aws_instance.instance_public_b.id}",
    "${aws_instance.instance_public_c.id}",
  ]
  
}
```
  
