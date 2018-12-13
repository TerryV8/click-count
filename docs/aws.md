# Deploy to AWS

## Setup 

Install the necessary requirement
```console
python --version   # version 2.7 at least
yum update
yum install python-pip
curl -O https://releases.hashicorp.com/terraform/0.11.10/terraform_0.11.10_linux_amd64.zip
mkdir /opt/terraform
unzip terraform_0.11.10_linux_amd64.zip -d /opt/terraform
PATH=$PATH:/opt/terraform
terraform --version
yum install ansible
ansible --version
```

Create an AWS client to communicate with AWS
```console
pip install awscli
aws --version
```

Generate the key to access the servers
```console
ssh-keygen
Enter file in which to save the key: /root/.ssh/kryptonite
ssh-agent bash  # allows to forwarding the key
ssh-add ~/.ssh/kryptonite  # add to that agent
ssh-add -l  # to check it is there

Modify the ansible configuration file, /etc/ansible/ansible.cfg
and we uncommented host_key_checking, 
which will prevent another error with aws when we try to access our server for the first time
```console
host_key_checking = False 
```

Create your own directory
```console
mkdir terransible
```

## IAM and DNS Setup, 

IAM and DNS are AWS items, which are not able to be configured through Terraform.

- IAM:
First, we give terraform, the permission that needs to provision all resources
Go to IAM console / Users / Add user 
=> Fill User name = terransible, Access type = Programmatic access
=> Next permissions / Attach existing policies directly => Check AdministratorAccess
=> Next review / Create User
=> Download .csv  # make sure to download the credencial

- DNS, with Route 53:
Register a domain name if need it

Add the IAM console credentials to our local server, so Terraform can do its job.
```console
aws configure --profile profile_terransible  # to create a new profile
AWS Access Key Id:  # Fill with the user credential AWS provided online 
AWS Secret Access Key:  
Default region name: eu-west-3  # for Paris Region, France
aws ec2 describe-instances --profile profile_terransible
{
    "Reservations": []
}

```



