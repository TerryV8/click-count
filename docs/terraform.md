# Terraform 

Terraform is an Infra provisioning tool which is cloud-agnostic. 
It is created by Hashicorp. 

Terraform defines your infrastructure as code. 
Its simple powerful syntax to describe infrastructure components 
allows you to build complex, version controlled, 
collaborative, heterogeneous and disposable systems with a very high productivity.

In some terms, “terraforming” begins with you describing the desired state of your infrastructure in a configuration file.
You then generate an execution ‘plan’ which describes various resources that will be created, 
modified and destroyed to reach the desired state.

You can then choose to ‘apply’ this plan, which will create actual resources.

## Setup

You can install terraform using Homebrew on a Mac: 
```console
brew update && brew install terraform.
terraform
    usage: terraform [--version] [--help] <command> [<args>]

    Available commands are:
        apply      Builds or changes infrastructure
        destroy    Destroy Terraform-managed infrastructure
        get        Download and install modules for the configuration
        graph      Create a visual graph of Terraform resources
        init       Initializes Terraform configuration from a module
        output     Read an output from a state file
        plan       Generate and show an execution plan
        pull       Refreshes the local state copy from the remote server
        push       Uploads the the local state to the remote server
        refresh    Update local state file against real resources
        remote     Configures remote state management
        show       Inspect Terraform state or plan
        version    Prints the Terraform version
```

## What we are going to build

We are going to build a private network on AWS
and establish a secure way to access network resources, using a trusted VPN.

We will build a Virtual Private Network (VPC) on AWS.
Instances in the private subnet cannot directly access the internet, making them an ideal for hosting critical resources such as application and our "Redis" database servers.

Instances in the private subnet rely on a Network Address Translation (NAT) server, running on the public subnet for internet connectivity. All instances in the public subnet can transmit inbound and outbound traffic to and from the internet.

To summarize, we will be building the below components:
- VPC
- Internet Gateway for public subnet
- Public subetn for routing instances
- Private subnet for internal resources
- Routing tables for public and private subnets
- NAT/VPN server to route outbound traffic from your instances in private network
and provide your workstation secure access to network resources.
- Application servers running nginx docker container in a private subnet
- Load balanvers in the public subnet to manage and route web traffic to app servers






