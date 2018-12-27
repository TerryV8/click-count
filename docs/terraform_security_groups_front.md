# Security group for the front

For the front, we are going to build our infrastructure based on this assumption:
- 1 security group allowing incoming HTTP from the Load Balancer to the Web Apps.
- The public subnet is accessible and all traffic (0.0.0.0/0) is routed directly to the Internet gateway
- 1 security group allowing incoming SSH and ICMP from a specific IP to all instances


Edit security_groups_front.tf:
```console

```
