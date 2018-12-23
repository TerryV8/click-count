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


# Our case with AWS

Here, Terraform provisiones a full AWS infrastructure from the ground using Terraform. 

To achieve this simple task, we will try to create a production-ready environment in AWS using Terraform automation which will require us to set up a VPC, Network Gateway, subnets, routes, security groups, EC2 machines with Redis installed inside the private network, and the web app in the public subnet.


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


