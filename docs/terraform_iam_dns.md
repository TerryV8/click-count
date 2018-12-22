# 1. What we are going to build

Here, we are going to build the below components:
- VPC
- Internet Gateway for public subnet
- Public subetn for routing instances
- Private subnet for internal resources
- Routing tables for public and private subnets
- NAT/VPN server to route outbound traffic from your instances in private network
and provide your workstation secure access to network resources. Instances in the private subnet rely on a Network Address Translation (NAT) server, running on the public subnet for internet connectivity. All instances in the public subnet can transmit inbound and outbound traffic to and from the internet.
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


# 3. On AWS, setup IAM

IAM is an AWS feature, that can only be configured through the AWS Web UI. IAM is going to give in our case, to terraform, the permission access to provision resources. 

On AWS Web UI:
- Go to IAM console / Users / Add user 
- => Fill User name = terransible, Access type = Programmatic access
- => Click on Next permissions / Attach existing policies directly => Check AdministratorAccess to attach the AdministratorAccess policy
- => Click on Next review / Create User
- => Download the credential .csv file # make sure to download the credencial

Thanks the credential .csv file,
we can retrieve the information on "AWS Access Key Id" and "AWS Secret Access Key"
that we can give to Terraform settings on our local machine.

In our case, we are going to create a new profile named "profile_terransible"

```console
aws configure --profile profile_terransible  # to create a new profile

    AWS Access Key Id: xxxxx # Fill with the AWS Access Key Id provided by credential .csv file,
    AWS Secret Access Key: xxxxx  # Fill with the AWS Secret Access Key provided by credential .csv file,
    Default region name: eu-west-3  # for Paris Region, France
```

To check that the client can now communicate with the AWS cloud, let us run this command
```console
aws ec2 describe-instances --profile profile_terransible

     ...
    {
        "Reservations": [...]
        
    }
    ...
    
```

# 4, On AWS, setup DNS for our hosted zone

After registering a domain name through "Route 53 DNS" on AWS Web UI, we need to set up a hosted zone
with Name servers (NS) that it will belong to. 

On our local machine, by typying the command "aws route 53 reusable-delegation-set", 
we can retrieve the name servers (NS) automatically configured for us.
The advantage is that it will allow us to set up any number of domain names that we wish, 
and still keep the same name servers (NS).


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

On AWS Web UI / Route 53 console / Registered domains / Domain name,
- => Add those NameServers generated below ("ns-1671.awsdns-16.co.uk", "ns-79.awsdns-09.com","ns-1052.awsdns-03.org", "ns-587.awsdns-09.net")
- => By Clicking on "Add or edit name servers". 

Thus, any of our domain name, and hosted zones will use those nameservers. 



# EXTRA
Generate the public/private key to securely access from our local machine to the AWS cloud servers
```console
ssh-keygen

    Enter file in which to save the key: /root/.ssh/kryptonite
    ssh-agent bash  # allows to forwarding the key
    ssh-add ~/.ssh/kryptonite  # add to that agent
    ssh-add -l  # to check it is there
```

Modify the ansible configuration file, /etc/ansible/ansible.cfg
by uncommented/putting host_key_checking = False
which will prevent errors with aws when we try to access our server for the first time
```console
host_key_checking = False 
```

Create your own directory
```console
mkdir terransible
```


### 1. What we are going to build

We are going to build a private network on AWS
and establish a secure way to access network resources, using a trusted VPN.

We will build a Virtual Private Network (VPC) on AWS.
Instances in the private subnet cannot directly access the internet, making them an ideal for hosting critical resources such as application and our "Redis" database servers.


