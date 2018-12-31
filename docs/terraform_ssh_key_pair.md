# Authentification by AWS SSH Key Pair

To login directly to the newly created instances, we can use the SSH key pair method.

Let us generate the new ssh key pair:
```console
keyname=clickcount-auth
keymail="thierry.vo@devoteam.com"
ssh-keygen -t rsa -b 4096 -f $keyname -C $keymail
```

Edit key-pairs.tf:

```console
resource "aws_key_pair" "auth"  {
  key_name = "clickcount-auth"
  public_key = "${file(var.public_key_directory_path)}/clickcount-auth"
}
```
