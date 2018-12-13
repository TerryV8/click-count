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
```

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

## IAM Setup

IAM is an AWS item, which is not able to be configured through Terraform, but relevant for Terraform

First, we give terraform, the permission that needs to provision all resources
Go to IAM console / Users / Add user 
=> Fill User name = terransible, Access type = Programmatic access
=> Next permissions / Attach existing policies directly => Check AdministratorAccess
=> Next review / Create User
=> Download .csv  # make sure to download the credencial



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

## DNS Setup

DNS is an AWS item, which is not able to be configured through Terraform, but relevant for Terraform

- DNS, with Route 53:
Register a domain name if need it

Let get information we need for our domain.
Let s create a reusable-delegation-set. It allows us to set up any number of domainnames that you wish but we keep the same name server that way we can run this script with any domain name that you want and you always be able to create a hosted zone.
Edit terraform/route53 and save the result of this command in this file: 
```console
aws route53 create-reusable-delegation-set --caller-reference 1224 --profile profile_terransible
    {
        "Location": "https://route53.amazonaws.com/2013-04-01/delegationset/N3NYEMYGEUSBJ6", 
        "DelegationSet": {
            "NameServers": [
                "ns-1671.awsdns-16.co.uk", 
                "ns-79.awsdns-09.com", 
                "ns-1052.awsdns-03.org", 
                "ns-587.awsdns-09.net"
            ], 
            "CallerReference": "1224", 
            "Id": "/delegationset/N3NYEMYGEUSBJ6"
        }
    }
```
Add those information to the Route 53 console/ registered domains. Click on your domain name (eg. mydomainname.com)
=> Click on "Add or edit name servers". Fill the nameservers that you just generated. Then click update. Then any hosted zones will use those nameservers. Click on Hosted zones to check





