# 3 EC2 instances for the front

The public subnet is accessible and all traffic (0.0.0.0/0) is routed directly to the Internet gateway

We will use our security group allowing incoming HTTP from the Load Balancer to the Web Apps.



Edit instance_public.tf :
```console
resource "aws_instance" "instance_public_a" {
  ami = "ami-051707cdba246187b"
  instance_type = "t2.micro"
  availability_zone = "${var.region}a"
  ebs_optimized = true
  subnet_id = "${aws_subnet.public-a.id}"
  security_groups = ["${aws_security_group.sg_LB_to_WebApps.id}", "${aws_security_group.sg_ssh_and_ping.id}"]
  associate_public_ip_address = true
  
  tags {
    Name = "instance_public_a"
  }
  
  block_device {
    device_name = "/dev/sda"
    volume_type = "gp2"
    volume_size = "300"
    delete_on_termination = "true"
  }
}

resource "aws_instance" "instance_public_b" {
  ami = "ami-051707cdba246187b"
  instance_type = "t2.micro"
  availability_zone = "${var.region}b"
  ebs_optimized = true
  subnet_id = "${aws_subnet.public-b.id}"
  security_groups = ["${aws_security_group.sg_LB_to_WebApps.id}", "${aws_security_group.sg_ssh_and_ping.id}"]
  associate_public_ip_address = true
  
  tags {
    Name = "instance_public_b"
  }
  
  block_device {
    device_name = "/dev/sda"
    volume_type = "gp2"
    volume_size = "300"
    delete_on_termination = "true"
  }
}

resource "aws_instance" "instance_public_c" {
  ami = "ami-051707cdba246187b"
  instance_type = "t2.micro"
  availability_zone = "${var.region}c"
  ebs_optimized = true
  subnet_id = "${aws_subnet.public-c.id}"
  security_groups = ["${aws_security_group.sg_LB_to_WebApps.id}", "${aws_security_group.sg_ssh_and_ping.id}"]
  associate_public_ip_address = true
  
  tags {
    Name = "instance_public_c"
  }
  
  block_device {
    device_name = "/dev/sda"
    volume_type = "gp2"
    volume_size = "300"
    delete_on_termination = "true"
  }
}




```
