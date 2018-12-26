# What we are going to build as "Infrastructure as Code"

Here, we are going to build the below components:
- VPC
- Internet Gateway for public subnet
- Public subnet for routing instances
- Private subnet for internal resources
- Routing tables for public and private subnets
- NAT/VPN server to route outbound traffic from your instances in private network
and provide your workstation secure access to network resources. Instances in the private subnet rely on a Network Address Translation (NAT) server, running on the public subnet for internet connectivity. All instances in the public subnet can transmit inbound and outbound traffic to and from the internet.
- Application servers running nginx docker container in a private subnet
- Load balanvers in the public subnet to manage and route web traffic to app servers
