# From a single host to multi-hosts (From Docker Compose to Kubernetes)

Difference between Docker compose and Kubernetes
One difference to note is that Docker Compose runs on a single host, whereas Kubernetes typically uses multiple nodes, which can be added or removed dynamically.


# Connection between containers (with Docker Compose)
Docker compose is a CLI utility used to connect containers with each other.


```console
version: '3.6'  
services:  
  app:
    build:
      context: ./app
    depends_on:
      - redis
    environment:
      - REDIS_HOST=redis
    ports:
      - "6379:6379"
  redis:
    image: redis:3.2-alpine
    volumes:
      - redis_data:/data
volumes:  
  redis_data:
```  
  
How to Read the Docker Compose File:
- We define two services, web and redis.
- The web service builds from the Dockerfile in the current directory
- Forwards the container’s exposed port (5000) to port 5000 on the host…
- Mounts the project directory on the host to /code inside the container (allowing you to modify the code without having to rebuild the image)…
- And links the web service to the Redis service.
- The redis service uses the latest Redis image from Docker Hub.





Alpine images

A lot of Docker images (versions of images) are created on top of Alpine Linux – this is a lightweight distro that allows you to reduce the overall size of Docker images.

I recommend that you use images based on Alpine for third-party services, such as Redis, Postgres, etc. For your app images, use images based on buildpack – it will be easy to debug inside the container, and you’ll have a lot of pre-installed system-wide requirements.

Only you can decide which base image to use, but you can get the maximum benefit by using one basic image for all images, because in this case the cache will be used more effectively.
  

Volume — can be described as a shared folder. Volumes are initialized when a container is created. Volumes are designed to persist data, independent of the container’s lifecycle. So, be careful with volumes. You should remember what data is in volumes. Because volumes are persistent and don’t die with the containers, the next container will use data from the volume created by the previous container.

We build and Run with Docker Compose.
Start the application from the current directory:
```console
docker-compose up --build
```

If you have localhost access to your host (i.e., you do not use a remote solution to deploy Docker), point your browser to http://0.0.0.0:5000, http://127.0.0.1:5000, or http://localhost:5000. On a Mac, you need to use docker-machine ip MACHINE_VM to get your Docker host’s IP address (then use that address like http://MACHINE_IP:5000 to access your web page). If you do use a remote host, simply use that IP address and append :5000 to the end.


# Kubernetes
Kubernetes is an open-source platform created by Google for container deployment operations, scaling up and down, and automation across the clusters of hosts. This production-ready, enterprise-grade, self-healing (auto-scaling, auto-replication, auto-restart, auto-placement) platform is modular, and so it can be utilized for any architecture deployment.

Kubernetes also distributes the load amongst containers. It aims to relieve the tools and components from the problem faced due to running applications in private and public clouds by placing the containers into groups and naming them as logical units. Their power lies in easy scaling, environment agnostic portability, and flexible growth.

# Application definition
Kubernetes: An application can be deployed in Kubernetes utilizing a combination of services (or microservices), deployments, and pods.

# Networking
Kubernetes: The networking model is a flat network, allowing all pods to interact with one another. The network policies specify how the pods interact with each other. The flat network is implemented typically as an overlay. The model needs two CIDRs: one for the services and the other from which pods acquire an IP address.

# Scalability
Kubernetes: For distributed systems, Kubernetes is more of an all-in-one framework. It is a complex system because it provides strong guarantees about the cluster state and a unified set of APIs. This slows down container scaling and deployment.

# High Availability
Kubernetes: All the pods in kubernetes are distributed among nodes and this offers high availability by tolerating the failure of application. Load balancing services in kubernetes detect unhealthy pods and get rid of them. So, this supports high availability.

# Container Setup
Kubernetes: Kubernetes utilizes its own YAML, API, and client definitions and each of these differ from that of standard docker equivalents. That is to say, you cannot utilize Docker Compose nor Docker CLI to define containers. While switching platforms, YAML definitions and commands need to be rewritten.

# Load Balancing
Kubernetes: Pods are exposed via service, which can be utilized as a load balancer within the cluster. Generally, an ingress is utilized for load balancing.

# What are we going to do

We could mount persistent volumes that would allow us to move containers without loosing data, it used flannel to create networking between containers, it has load balancer integrated, it uses etcd for service discovery, and so on. However, Kubernetes comes at a cost. It uses a different CLI, different API and different YAML definitions. In other words, you cannot use Docker CLI nor you can use Docker Compose to define containers.



