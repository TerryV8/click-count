
The basic configuration in the /etc/hosts in a docker instance of a pod:
would look like this:
<img src="/images/etc_hosts_in_pods.png" width="50%">



Since we need that this docker instance need to communicate with the redis server,
we will map the redis server with the HAProxy, which will forward the traffic to the master od the redis cluster.

So, ideally, we would append this to the /etc/hosts in a docker instance of a pod:
```console
redis 10.211.55.4
```
where 10.211.55.4 is the HAProxy IP.

However, a better approach to automate is to edit the deployment file of Kubernetes, deployment-web-app.yml :
```console
apiVersion: v1
kind: Deployment
metadata:
  name: web-app
  labels:
    app: web-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      hostAliases:
      - ip: "10.211.55.4"
        hostnames:
        - "redis"
      containers:
        - image: thierrylamvo/web-app
          name: web-app
          ports:
          - containerPort: 80
          imagePullPolicy: Always
```
          
We especially added those lines which will append the /etc/hosts on each pod to map the redis server ip with the ip 10.211.55.4, which is fixed.
```console
      hostAliases:
      - ip: "10.211.55.4"
        hostnames:
        - "redis"
```
        
        
        
