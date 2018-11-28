# Scalalibity (With Kubernetes)

## We create our pods

Edit pod.yml: 
```console
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod-demo
  labels:
    app:nginx-demo
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

