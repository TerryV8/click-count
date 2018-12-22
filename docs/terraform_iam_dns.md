# 1. What we are going to build

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



# 2. Local Setup 

On your local machine, install:
- terraform 
- ansible
- AWS client to communicate with AWS cloud
and the AWS client to connect to AWS
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

pip install awscli
aws --version
```

#Generate the public/private key to securely access from our local machine to the AWS cloud servers
#```console
#ssh-keygen

#    Enter file in which to save the key: /root/.ssh/kryptonite
#    ssh-agent bash  # allows to forwarding the key
#    ssh-add ~/.ssh/kryptonite  # add to that agent
#    ssh-add -l  # to check it is there
#```

#Modify the ansible configuration file, /etc/ansible/ansible.cfg
#by uncommented/putting host_key_checking = False
#which will prevent errors with aws when we try to access our server for the first time
#```console
#host_key_checking = False 
#```

#Create your own directory
#```console
#mkdir terransible
#```

# 3. On AWS, setup IAM

IAM is an AWS item, which is not able to be configured through Terraform

We are going to give terraform, the permission that needs to provision resources
Thus, go to IAM console / Users / Add user 
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

# 4, On AWS, setup DNS

DNS is an AWS item, which is not able to be configured through Terraform

- DNS, with Route 53:
Route 53 / Register a domain.


Let's get information we need for our domain.
Let's create a reusable-delegation-set. It allows us to set up any number of domain names that we wish, but we keep the same name server. That way we can work with any domain name that we want and we are always able to create hosted zones.
Open terraform/route53 and save the result of this command in this file: 
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
Add those information to the Route 53 console / Registered domains. Click on your domain name (eg. mydomainname.com)
=> Click on "Add or edit name servers". Fill the nameservers that you just generated. Then click update. Then any hosted zones will use those nameservers. Click on Hosted zones to check





