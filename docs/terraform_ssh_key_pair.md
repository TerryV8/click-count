# Create SSH Key Pair

We will need an SSH key to be bootstrapped on the newly created instances to be able to login. Make sure you have the ssh directory and generate a new key by running:

```console
ssh-keygen -t rsa -C "insecure-deployer" -P '' -f ssh/insecure-deployer
```

The above command will create a public-private key pair in the ssh directory. This is an insecure key and should be replaced after the instance is bootstrapped.

Create a new file key-pairs.tf with the below configuration and register the newly generated SSH key pair by runningterraform plan and terraform apply.

```console
resource "aws_key_pair" "deployer"  {
  key_name = "deployer-key"
  public_key = "${file(\"ssh/insecure-deployer.pub\")}"

}
```
