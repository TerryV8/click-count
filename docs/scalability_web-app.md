# Scalable: increasing throughput: Deploying the containerized web application

We are going to package the web application in a Docker container image, and run that container image on the Kubernetes cluster as a load-balancer set of replicas that can scale to the needs of our users.

# Objective
To package and deploy our application on Kubernetes cluster, we must:
1. Package the application the app in Docker image
2. Run the container locally on our machine
3. Upload the image to the registry
4. Create a container cluster
5. Deploy our app to the cluster
6. Expose our app to the Internet
7. Scale up our deployment
8. Deploy a new version of our app


## 1. Package the application the app in Docker image

To build a Docker image, you need to have an application and a Dockerfile.
The application is packaged as a Docker image, using the Dockerfile that contains instructions on how the image is built. 

We did it in the section
- [Jenkins](docs/continuous_integration.md)
- [Docker, for front-end](docs/docker_front-end.md)


## 2. Run the container locally on our machine

We can run the container locally if we want to
for testing

```console
docker run --name web-app -p 8080:8080 -d --link=redis terryv8/web-app
```

## 3. Upload the container image to the registry

To upload it:
```console
docker login
```

Don't forget to push 


## 4. Create a container cluster

Our cluster consists of a pool of VM instances running Kubernetes, the open source cluster orchestration system. 
Once you have created the Kubernetes cluster, you let Kubernetes manage the applications’ lifecycle.

We do this in this section:
  - [Kubernetes: Installation for Resilience: auto-healing containers, no failover ](docs/replication.md)


## 5. Deploy our app to the cluster

To communicate with the Kubernetes cluster, you typically do this by using the kubectl command-line tool.

Kubernetes represents applications as Pods, which are units that represent a container (or group of tightly-coupled containers). 

The kubectl run command below causes Kubernetes to create a Deployment named web-app on your cluster. The Deployment manages multiple copies of your application, called replicas, and schedules them to run on the individual nodes in your cluster. In this case, the Deployment will be running only one Pod of your application.

Run the following command to deploy your application, listening on port 8080:
```console
kubectl run web-app --image=terryv8/web-app --port 8080
```

To see the Pod created by the Deployment, run the following command:
```console
kubectl get pods
```

Output
```console

```





Expose our app to the Internet
Scale up our deployment
Deploy a new version of our app

## We create our pods

Edit pod.yml: 
```console
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod-demo
  labels:
    app: nginx-demo
spec:
  containers:
    - image: nginx:latest
      name: nginx-demo
      ports:
      - containerPort: 80
      imagePullPolicy: Always
```

As a root user, launch the command:
```console
kubectl create -f pod.yml
```

Check the pods created by:
```console
kubectl get pods
```

The ouput should look like this:
```console
NAME             READY     STATUS              RESTARTS   AGE
nginx-pod-demo   0/1       ContainerCreating   0          26s
```

After a few seconds:
```console
NAME             READY     STATUS    RESTARTS   AGE
nginx-pod-demo   1/1       Running   0          1m
```


## We create our services
Edit service.yml: 
```console
apiVersion: v1
kind: Service
metadata:
  name: service-demo
spec:
  selector:
    app: nginx-demo
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: NodePort
```

As a root user, launch the command:
```console
kubectl create -f service.yml
```

Check the pods created by:
```console
kubectl get services
```

The ouput should look like this:
```console
NAME           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes     ClusterIP   10.96.0.1       <none>        443/TCP        1d
service-demo   NodePort    10.105.95.155   <none>        80:32334/TCP   57s

```



## We create our deployment to scale up/down:

Edit deployment.yml: 
```console
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redisServices
  labels:
    app: redisServices
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redisServices
  template:
    metadata:
      labels:
        app: nginx-demo
    spec:
      containers:
        - image: TerryV8/click-count
          name: redisServices
          envFrom:
          - configMapRef:
            name: controller-config
          env:
            - name: TIER_NAME
              value: redisServices
          ports:
          - containerPort: 80
          imagePullPolicy: Always
      restartPolicy: Always
      
```
As we mentioned earlier, networking is done differently in Kubernetes than in Docker Compose. The Service is what enables communication between containers. Here is the service definition for quoteServices:

```console
apiVersion: v1
kind: Service
metada:
  labels:
    name: redisServices
  name: redisServices
spec:
  ports:
  - name: "8080"
    port: 8080
    targetPort: 8080
  selector:
    name: redisServices
status:
  loadBalancer: {}
```
    

Edit deployment.yml: 
```console
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-pod-demo-deployment
  labels:
    app: nginx-demo
spec:
  replicas: 5
  selector:
    matchLabels:
      app: nginx-demo
  template:
    metadata:
      labels:
        app: nginx-demo
    spec:
      containers:
        - image: nginx:latest
          name: nginx-demo
          ports:
          - containerPort: 80
          imagePullPolicy: Always
```

As a root user, launch the command:
```console
kubectl create -f deployment.yml
```

Check the deployment created by:
```console
kubectl get deployment
```

The ouput should look like this:
```console
NAME                        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx-pod-demo-deployment   5         5         5            3           26s

```

Check the pods created by:
```console
kubectl get pods
```

The ouput should look like this:
```console
NAME                                         READY     STATUS    RESTARTS   AGE
nginx-pod-demo-deployment-675fbcc4c4-7wsf5   1/1       Running   0          3m
nginx-pod-demo-deployment-675fbcc4c4-crrt7   1/1       Running   0          3m
nginx-pod-demo-deployment-675fbcc4c4-q9khc   1/1       Running   0          3m
nginx-pod-demo-deployment-675fbcc4c4-w2vh9   1/1       Running   0          3m
nginx-pod-demo-deployment-675fbcc4c4-zjzss   1/1       Running   0          3m
```

### To scale up

For example to scalep up to 20,
Edit deployment.yml and replace the line: "replicas: 5"
by "replicas: 20"
  


As a root user, launch the command to verify:
```console
kubectl apply -f deployment.yml
```

The ouput should look like this:
```console
NAME                                         READY     STATUS              RESTARTS   AGE
nginx-pod-demo-deployment-675fbcc4c4-5s6h9   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-9fk8t   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-9p2cj   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-cttq7   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-dswg4   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-dw8hx   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-h8r5l   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-jlnh2   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-k78ch   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-ld9wq   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-ldmql   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-n5qvx   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-q6cdb   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-q9khc   1/1       Running             0          10m
nginx-pod-demo-deployment-675fbcc4c4-qxzbc   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-rmcp4   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-rnb2z   0/1       ContainerCreating   0          36s
nginx-pod-demo-deployment-675fbcc4c4-sscvx   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-tgm4b   1/1       Running             0          36s
nginx-pod-demo-deployment-675fbcc4c4-zqnsj   1/1       Running             0          36s
```



### To scale down

For example to scalep up to 1,
Edit deployment.yml and replace the line: "replicas: 5"
by "replicas: 1"
  


As a root user, launch the command to verify:
```console
kubectl apply -f deployment.yml
```

The ouput should look like this:
```console
NAME                                         READY     STATUS    RESTARTS   AGE
nginx-pod-demo-deployment-675fbcc4c4-q9khc   1/1       Running   0          9m
```

# Load Balancing
If you go the url of one of the slave nodes,
for example 10.211.55.5:32334,
you will see that you are able to use the service



