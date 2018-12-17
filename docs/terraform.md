# Terraform 

Terraform is an automation deploying tool on the cloud, 
from Hashicorp (Creators of Vagrant, Consul
and many more automation favorites).


Terraform defines your infrastructure as code. 
Its simple powerful syntax to describe infrastructure components 
allows you to build complex, version controlled, 
collaborative, heterogeneous and disposable systems with a very high productivity.

In simple terms, “terraforming” begins with you describing the desired state of your infrastructure in a configuration file.
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





