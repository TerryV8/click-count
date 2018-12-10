

# Using Kubernetes, we decided to go for a Redis Master-Slaves architecture

There is a master and there are replicas. The master pushes data to replicas. Replicas don’t talk between themselves.
Replicas are Read-Only.

![Redis replicated architecture](https://blog.octo.com/wp-content/uploads/2017/08/screen-shot-2017-08-11-at-14-35-11.png)


- Pros:
  - You always have a hot snapshot (permanent store) of your data
  - It’s a Disaster Recovery Plan (DRP) with Master - Slave Stand-By
- Cons:
  - Write performance is bounded by the master
  - The replicas are not used as write node, only read node. 


# Step 1: Set up a Redis master

The guestbook application uses Redis to store its data. It writes its data to a Redis master instance and reads data from multiple Redis worker (slave) instances. The first step is to deploy a Redis master.

Use the manifest file named redis-master-deployment to deploy the Redis master. This manifest file specifies a Deployment controller that runs a single replica Redis master Pod:


Edit deployment-redis-master.yml:
```console
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: redis-master
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
        role: master
        tier: backend
    spec:
      containers:
      - name: master
        image: k8s.gcr.io/redis:e2e  # or just image: redis
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 6379
```

Run the following command to deploy the Redis master:
```console
kubectl create -f deployment-redis-master.yml
```

Verify that the Redis master Pod is running kubectl get pods:
```console
kubectl get pods
```

Output:
```console

```




