# Internet gateway and a Route table for routing

Let us edit routing.tf:
```console
resource "aws_internet_gateway" "gw-to-internet" {
  vpc_id = "${aws_vpc.default.id}"
}

resource "aws_route_table" "route-to-gw" {
  vpc_id = "${aws_vpc.default.id}"
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = "${aws_internet_gateway.gw-to-internet.id}" 
  }
}
```


When we go on the AWS Web UI Console,
and check the newly route table "route-to-gw":
```console
Destination         Target                    Status      Propagated
10.123.0.0/16         local	                    active      No	
0.0.0.0/0         igw-0a16112bf05f1795c	    active    No
```
shows us that the destination 10.123.0.0/16 will be routed to the local (IP range of VPC)
and the other will be forward to the internet gateway


