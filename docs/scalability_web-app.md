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
- => [Jenkins](docs/continuous_integration.md)
- => [Docker, for front-end](docs/docker_front-end.md)


## 2. Run the container locally on our machine

We can run the container locally if we want to
for testing

```console
docker run --name web-app -p 8080:8080 -d --link=redis thierrylamvo/web-app
```

## 3. Upload the container image to the registry

Docker Hub is the place where open Docker images are stored.

Log into the Docker Hub from the command line
```console
docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username (thierrylamvo): 
```

Push your image to the repository you created
```console
docker push thierrylamvo/web-app
```


## 4. Create a container cluster

Our cluster consists of a pool of VM instances running Kubernetes, the open source cluster orchestration system. 
Once you have created the Kubernetes cluster, you let Kubernetes manage the applications’ lifecycle.

We do this in this section:
  - => [Kubernetes: Installation for Resilience: auto-healing containers, no failover ](docs/replication.md)


## 5. Deploy our app to the cluster with Kubernetes pods

To communicate with the Kubernetes cluster, you typically do this by using the kubectl command-line tool.

Kubernetes represents applications as Pods, which are units that represent a container (or group of tightly-coupled containers). 

The kubectl run command below causes Kubernetes to create a Deployment named web-app on your cluster. The Deployment manages multiple copies of your application, called replicas, and schedules them to run on the individual nodes in your cluster. In this case, the Deployment will be running only one Pod of your application.

> A possible approach, but not recommanded way to deploy our application, listening on port 8080 is to launch this command:
> ```console
> kubectl run web-app --image=thierrylamvo/web-app --port 8080
> ```

However, a better approach is to Edit pod-web-app.yml :
```consoleapiVersion: v1
kind: Pod
metadata:
  name: web-app
  labels:
    app: web-app
spec:
  containers:
    - image: thierrylamvo/web-app
      name: web-app
      ports:
      - containerPort: 80
      imagePullPolicy: Always
```

To see the Pod created by the Deployment, run the following command:
```console
kubectl get pods
```
Then launch the command as a root user, :
```console
kubectl create -f pod-web-app.yml
```

Check the pod is launching by:
```console
kubectl get pods
```

Output
```console
NAME                           READY     STATUS    RESTARTS   AGE
web-app                        1/1       Running   0          23s
```


## 6. Expose our app to the Internet with Kubernetes services

By default, the containers you run are not accessible from the Internet, because they do not have external IP addresses. You must explicitly expose your application to traffic from the Internet

> A possible approach, but not recommanded way to expose your application to traffic from the Internet, listening on port 8080 is to launch this command:
> ```console
> kubectl expose deployment web-app --type=LoadBalancer --port 80 --target-port 8080
> ```
> The kubectl expose command above creates a Service resource, which provides networking and IP support to your application's Pods. If you were on a cloud such as AWS, GKE, it will create an external IP and a Load Balancer (subject to billing) for your application.
> The --port flag specifies the port number configured on the Load Balancer, and the --target-port flag specifies the port number that is used by the Pod created by the kubectl run command from the previous step.


However, a better approach is to Edit service-web-app.yml :
```console
apiVersion: v1
kind: Service
metadata:
  name: web-app
spec:
  selector:
    app: web-app
  ports:
  - protocol: TCP
    port: 8080
    targetPort: 8080
    NodePort: 30808
  type: NodePort
```

Keep in mind that NodePort must be set to a number in the flag-configured range range 30000-32767. Otherwise, kubernetes throws an error. This NodePort range can be changed using the flag --service-node-port-range passed to kube-apiserver per https://kubernetes.io/docs/reference/generated/kube-apiserver/:


Then launch the command as a root user, :
```console
kubectl create -f service-web-app.yml
```

Check the pod is launching by:
```console
kubectl get services
```

Output
```console
NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
web-app        NodePort    10.109.242.242   <none>        8080:30808/TCP   22m
```

Usually, on a cloud, EXTERNAL-IP will be generated automatically. Since we're on local VM, we will need to do some extra tasks to expose to the external world.

Once you've determined the external IP address for your application, copy the IP address. Point your browser to this external IP URL (eg. http://203.0.113.0) to see that your application will be accessible.



## 7. Scale up our deployment with Kubernetes deployment

To manage the increasing throughput on our web-app service, we need to scale up.
In the opposite, to reduce the cost because of decreasing throughput on our web-app service, we need to scale down.

Thus, now, we are going to learn how to scale up, by adding more replicas to our web-app's Deployment resource by using the kubectl scale command. To add two additional replicas to your Deployment (for a total of three), run the following command:

> A possible approach, but not recommanded way to scale up our application is to launch this command:
> ```console
> kubectl scale deployment hello-web --replicas=3
> ```

However, a better approach is to Edit deployment-web-app.yml :
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
      containers:
        - image: thierrylamvo/web-app
          name: web-app
          ports:
          - containerPort: 80
          imagePullPolicy: Always
```

Then launch the command as a root user, :
```console
kubectl create -f deployment-web-app.yml
```

Check the pod is launching by:
```console
NAME          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
web-app       2         2         2            2           36s
```

Now, you can check that your app is well deployed on the other nodes of the Kubernetes cluster:
```console
kubectl get pods   # allows us to check the pods running
```

Output
```console
NAME                           READY     STATUS    RESTARTS   AGE
web-app-64d996d646-5nbqz       1/1       Running   0          4m
web-app-64d996d646-ndnnw       1/1       Running   0          4m
```

To check the IP address of the nodes where the pods where deployed:
```console
kubectl describe pods web-app-64d996d646-5nbqz
```

OUTPUT
```console
Name:               web-app-64d996d646-5nbqz
Namespace:          default
Priority:           0
PriorityClassName:  <none>
Node:               centos-linux-slave-1.shared/10.211.55.5
Start Time:         Sun, 09 Dec 2018 16:36:16 +0100
Labels:             app=web-app
                    pod-template-hash=2085528202
Annotations:        <none>
Status:             Running
IP:                 10.244.1.55
Controlled By:      ReplicaSet/web-app-64d996d646
Containers:
  web-app:
    Container ID:   docker://05c2463f2192fc54999198179d040d6b9d948d314d31ef205c686bd264115777
    Image:          thierrylamvo/web-app
    Image ID:       docker-pullable://docker.io/thierrylamvo/web-app@sha256:56c508daa62fdf12d728fe6d78c6f1730ddce10ead06284acbeb867ddca6759c
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sun, 09 Dec 2018 16:36:19 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-bjn4p (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-bjn4p:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-bjn4p
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From                                  Message
  ----    ------     ----  ----                                  -------
  Normal  Scheduled  6m    default-scheduler                     Successfully assigned default/web-app-64d996d646-5nbqz to centos-linux-slave-1.shared
  Normal  Pulling    6m    kubelet, centos-linux-slave-1.shared  pulling image "thierrylamvo/web-app"
  Normal  Pulled     6m    kubelet, centos-linux-slave-1.shared  Successfully pulled image "thierrylamvo/web-app"
  Normal  Created    6m    kubelet, centos-linux-slave-1.shared  Created container
  Normal  Started    6m    kubelet, centos-linux-slave-1.shared  Started container
```

To check the IP address of the second node where the pods where deployed;
```console
kubectl describe pods web-app-64d996d646-ndnnw
```

OUTPUT
```console
Name:               web-app-64d996d646-ndnnw
Namespace:          default
Priority:           0
PriorityClassName:  <none>
Node:               centos-linux-slave-2.shared/10.211.55.6
Start Time:         Sun, 09 Dec 2018 16:36:16 +0100
Labels:             app=web-app
                    pod-template-hash=2085528202
Annotations:        <none>
Status:             Running
IP:                 10.244.2.38
Controlled By:      ReplicaSet/web-app-64d996d646
Containers:
  web-app:
    Container ID:   docker://35c67ac9b152f44268575996868b152de87ba2444efc3e77e2a40d75e41a9b4f
    Image:          thierrylamvo/web-app
    Image ID:       docker-pullable://docker.io/thierrylamvo/web-app@sha256:56c508daa62fdf12d728fe6d78c6f1730ddce10ead06284acbeb867ddca6759c
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sun, 09 Dec 2018 16:36:19 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-bjn4p (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-bjn4p:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-bjn4p
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age   From                                  Message
  ----    ------     ----  ----                                  -------
  Normal  Scheduled  9m    default-scheduler                     Successfully assigned default/web-app-64d996d646-ndnnw to centos-linux-slave-2.shared
  Normal  Pulling    9m    kubelet, centos-linux-slave-2.shared  pulling image "thierrylamvo/web-app"
  Normal  Pulled     9m    kubelet, centos-linux-slave-2.shared  Successfully pulled image "thierrylamvo/web-app"
  Normal  Created    9m    kubelet, centos-linux-slave-2.shared  Created container
  Normal  Started    9m    kubelet, centos-linux-slave-2.shared  Started container
```

As you can see from the ouput above,
our pods where well distributed equally on the slave nodes of Kubernetes cluster.

# Load Balancing
If you go the url of one of the slave nodes,
for example 10.211.55.5:32334,
you will see that you are able to use the service


## 8. Deploy a new version of our app





TO DELETE


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



