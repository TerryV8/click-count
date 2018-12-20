# Security groups

We will be creating 3 security groups:

- default: default security group that allow inbound and outbound traffic from all instances in the VPC
- nat: security group for NAT instances that allow SSH traffic from the internet
- web: security group that allows web traffic from the internet

Edit security-groups.tf:
```console
/* Default security group */


/* Security group for the nat server */


/* Security group for the web */
resource "aws_security_group" "web" {
  name = "web-airpair-example"
  description = "Security group for web that allows web traffic from internet"
  vpc_id = ${aws_vpc.default.id}
}
```

Run terraform plan to review your changes and then run terraform apply. You should see an output like this:
```console
Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
```

