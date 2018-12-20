# Create the NAT Instance

NAT instances reside in the public subnet.
In order to route traffic, they need to have the 'source destination check' parameter disabled.
They belong to the default and nat security groups. The default security group allows traffic from any instance within the group.
The nat security group allows SSH and VPN traffic from the internet

Edit nat-server.tf:
```console


```
