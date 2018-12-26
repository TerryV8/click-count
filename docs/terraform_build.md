# What we are going to build as "Infrastructure as Code"

The build of the infrastructure through Terraform will be done in 5 steps:
- Presets: Variables et Credentials
- Network:
  - VPC
  - Public subnet for routing instances
  - Internet Gateway in public subnet
  - Private subnet for internal resources
- Firewall: Security Groups
- Servers: 
  - EC2 of Application servers running docker of glashfish servers
  - ELB in the public subnet to route web traffic to application servers 
  - EC2 of Redis for storage
- DNS: Route53


- NAT/VPN server to route outbound traffic from your instances in private network
and provide your workstation secure access to network resources. Instances in the private subnet rely on a Network Address Translation (NAT) server, running on the public subnet for internet connectivity. All instances in the public subnet can transmit inbound and outbound traffic to and from the internet.
- Application servers running nginx docker container in a private subnet
- Load balanvers in the public subnet to manage and route web traffic to app servers
