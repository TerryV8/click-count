# Ansible

Configuration Management vs Orchestration
Chef, Puppet & Ansible are all configuration management tools and are designed to install and manage softwares on existing servers. Terraform & CloudFormation are orchestration tools and are designed to provision the servers themselves, leaving the job to configure these servers to some other tools. Orchestration is the process of combining multiple automation tasks to create an instance or IP etc. While most of the configuration management tools can do some degree of orchestration and vice-versa, they lack the depth & breadth of specialized tools like Terraform.
With Docker, however configuration management has become less complex and simple to manage in itself as you can create docker images with pre-installed softwares. All you need after creating the docker image is a server to run it. That is where Terraform can be a better fit than a configuration management tool like Chef or Puppet or Ansible.
