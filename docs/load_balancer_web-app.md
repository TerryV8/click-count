# HAProxy 

We are going to install and configure HAProxy on Centos 7.
HAProxy is a free HTTP/TCP high availability load balancer and proxy server.
It spreads requests among multiple servers to mitigate issues resulting from single server failure. 
HA Proxy is used by a number of high-profile websites including GitHub, Bitbucket, Stack Overflow, Reddit, Tumblr, Twitter and Tuenti
and is used in the OpsWorks product from Amazon Web Services.

# Install HAProxy 
Since we have on 3 local VMs.
Below is our network server. There are 2 web servers running our Web App and listening on port 30808 
and one HAProxy server:

- Web App Server 1:  10.211.55.5
- Web App Server 2:  10.211.55.6
- HAProxy Server:    10.211.55.4
  
Now install HAProxy with the following command:
```console
yum install haproxy
```

Modify the configuraion file of haproxy, /etc/haproxy/haproxy.cfg as per our requirement
When you configure load balancing using HAProxy, there are two types of nodes which need to be defined: frontend and backend. 
The frontend is the node by which HAProxy listens for connections. Backend nodes are those by which HAProxy can forward requests. A third node type, the stats node, can be used to monitor the load balancer and the other two nodes.

Append those lines to /etc/haproxy/haproxy.cfg
```console
#---------------------------------------------------------------------
# clickCount - Homework
#---------------------------------------------------------------------

frontend haproxywebappclickcount
    bind *:8080
    mode http
    default_backend backendwebappclickcount

backend backendwebappclickcount
    balance roundrobin
    server node1 10.211.55.5:30808/clickCount check
    server node2 10.211.55.6:30808/clickCount check
```

This configures a frontend named haproxywebappclickcount, which handles all incoming traffic on port 8080.
The default_backend backendwebappclickcount specifies that all other traffic will be forwarded to backendwebappclickcount and maps to the port 30808

Now, any incoming requests to the HAProxy node at IP address 10.211.55.4 will be forwarded to an internally networked node with an IP address of either 10.211.55.5 or 10.211.55.6, also it maps port 8080 to port 30808. These backend nodes will serve the HTTP requests. If at any time either of these nodes fails the health check, they will not be used to serve any requests until they pass the test.




