# Terraform 

Terraform is an infrastructure automation tool created by Hashicorp. 
It is a tool used for building, changing, versioning and destroying infrastructure and its resources such as compute instances, storage, networking, load balancers DNS etc.

It defines your infrastructure as code and it is cloud-agnostic. Terraform allows you to define the infrastructure for as simple as a server to a complete data center through configuration scripts written in HCL, from which it creates an execution plan depicting what it will do to achieve the desired state, and afterwards executes it to construct the create it, modified or destroyed to reach the desired state of the infrastructure. 

# Our case with AWS

Here, Terraform provisions a full AWS infrastructure from the ground using Terraform. 

In our case, we are going to create a production-ready environment in AWS using Terraform automation which will require us to set up a VPC, Network Gateway, subnets, routes, security groups, EC2 machines with Redis installed inside the private network, and the web app in the public subnet.


## Setup

You can install terraform using Homebrew on a Mac. Then verify, terraform is well installed.
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


