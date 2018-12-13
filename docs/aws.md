# Deploy to AWS

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
