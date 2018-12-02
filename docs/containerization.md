# Docker

Why do we need Docker?
The short list of benefits includes:

- faster development process
- handy application encapsulation
- the same behaviour on local machine / dev / staging / production servers
- distribute the whole app inclusing the "server" in a ready-to-run state
- easy and clear monitoring
- easy to scale. Automation to provision
- better than Virtual Machine (VM)


## Faster development process
There is no need to install 3rd-party apps like PostgreSQL, Redis, or Elasticsearch on the system — you can run them in containers.

Docker also gives you the ability to run different versions of same application simultaneously. For example, say you need to do some manual data migration from an older version of Postgres to a newer version. You can have such a situation in microservice architecture when you want to create a new microservice with a new version of the 3rd-party software.

It could be quite complex to keep two different versions of the same app on one host OS. In this case, Docker containers could be a perfect solution — you receive isolated environments for your applications and 3rd-parties.



## Handy application encapsulation

With Docker you can easily deploy a web application along with it’s dependencies, environment variables, and configuration settings - everything you need to recreate your environment quickly and efficiently.

You can deliver your application in one piece. Most programming languages, frameworks, and all operating systems have their own packaging managers. And even if your application can be packed with its native package manager, it could be hard to create a port for another system.

Docker gives you a unified image format to distribute your applications across different host systems and cloud services. You can deliver your application in one piece with all the required dependencies (included in an image) ready to run.

Same behaviour on local machine / dev / staging / production servers
Docker can’t guarantee 100% dev / staging / production parity, because there is always the human factor. But it reduces to almost zero the probability of error caused by different versions of operating systems, system-dependencies, and so forth.

With the right approach to building Docker images, your application will use the same base image with the same OS version and the required dependencies.



## Easy and clear monitoring

Out of the box, you have a unified way to read log files from all running containers. You don’t need to remember all the specific paths where your app and its dependencies store log files, and write custom hooks to handle this. 
You can integrate an external logging driver and monitor your app log files in one place.



## Easy to scale

A correctly wrapped application will cover most of the Twelve Factors. By design, Docker forces you follow its core principles, such as configuration over environment variables, and communication over TCP/UDP ports. And if you’ve done your application right, it will be ready for scaling not only in Docker.

## Better than Virtual Machine (VM)

Even if Virtual Machine is a virtualization tools that helps us to spin up new production servers when we need to
compare to traditional architecture.

Virtual Machine are nowadays:
- slower
- using a lot of resources
- not portable
- not offering that the code is deployed in the same way on all the VM
compare to the containarization offered by Docker

What if we have something like a kind of VM, but faster, smaller and easier to work with than a VM. That's what Docker offered. We could package the code and the system-level configuration (entire OS, and simulations of all the hardware) in a lightweight package that can stand up quickly and run almost anywhere.

It is lightweight, because container has only what the app needs in order to run. Moreover, orchestration becomes much easier with container thanks to its lightweight.



# Docker-specific terms
- A Dockerfile is a file that contains a set of instructions used to create an image.
- An image is used to build and save snapshots (the state) of an environment.
- A container is an instantiated, live image that runs a collection of processes.



# Install docker
```console
sudo yum -y install docker
sudo groupadd docker
sudo usermod -aG docker user
sudo systemctl start docker
sudo systemctl enable docker
```

In production, run the container with the restart policy,
docker eill actually take care of making sure that the container is always running
if the application inside the container crashes, docker will automatically recreate the container.
If the server itself restart or if the docker service restart, docker will make sure that it stands up that container again.
As long as we explicitly don't stop the container, docker makes that the container is always running.
That's all we need to do to actually to do to get our application to run on production

```console
docker run --restart always
```


# Check docker is running
```console
docker pull docker.io/hello-world
docker images
docker run hello-world
docker ps -a
```

# Create DockerFile and save it on the Hub

An image is defined in a Dockerfile and then create using the "docker build" command
When you build, you give your image a name (and possibly tags):
```console
docker build -t <docker username>/<image-name> .
```

Once you build the image, you can create and run a container instance:
```console
docker run -d <docker username>/<image-name> 
```

To see running containers:
```console
docker ps
```

Once you "docker build" an image, you can "docker push" it to registry.
Then you can "docker run" that image from anywhere that is set up to access that registry.
You can maintain your own private registries or you can use the official cloud registry, Docker Hub (hub.docker.com)
To authenticate with Docker Hub:
```console
docker login
```



# Dockerfile

The Dockerfile defines the docker images that will be built. It consists of a series of instructions for produing the image:
- FROM, sets the parent image that I want to build the image from
- WORKDIR, sets the current working directory inside the container image for other commands where the source will be put at
- COPY, copies files from the host into the container image, eg. copy all the source code that is in folder that I cloned from my github and then i am going to copy into the current directory inside the image
- RUN, executes a command within the container image
- EXPOSE, tells docker that the application in the container listens on a particular ports
- CMD, sets the command that is executed by the container when it is run. Whenever I run a container based on that image, it is going to the run the command inside the working directory


```console
git clone https://github.com/<your git username>/<your git repository>
cd <your git repository> 
vi Dockerfile

```console
docker login
docker push
```

```console
FROM node:carbon
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD ["npm", "start"]
```

Build a docker image using the dockerfile
```console
docker build -t TerryV8/click-count .
```

To have a runnable image with -p to specify the port mapping between the port of my host and the port inside my container,
 -d for a detached mode.
 ```console
 docker run -p 8080:8080 -d TerryV8/clickcount
 ```
 
If you got the warning message after the "docker run":
```console
WARNING: IPv4 forwarding is disabled. Networking will not work.
```

Edit /etc/sysctl.d/enable-ip-forward.conf:
```console
net.ipv4.ip_forward=1
```

Then restart the network service:
```console
sudo systemctl restart network
```


# Running Redis in a Docker Container

The official Redis image contains the command EXPOSE 6379 (the default Redis port) which makes it automatically available to any linked containers.
To run a Redis instance in a Docker container named my-redis-container, use the command:

```console
sudo docker run --name my-redis-container -d redis 
```


# Connect to a Redis Container From a Remote Server
If you wish to connect to a Docker container running Redis from a remote server, you can use Docker's port forwarding to access the container with the host server's IP address or domain name.

To use Docker's port forwarding for Redis, add the flag -p [host port]:6379 to the docker run command.

For example, to set up port forwarding so that you can connect to the container using port 7001, the docker run command is:
```console
sudo docker run --name my-redis-container -p 7001:6379 -d redis
```

You can then switch to another server and access the my-redis-container container with the command:
```console
sudo redis-cli -h [host IP or domain name] -p 7001
```

For example, if the host server running the Redis container is IP address 192.168.0.1, you can access the Redis container from any server with the command:
```console
sudo redis-cli -h 192.168.0.1 -p 7001
```


# Test 1
Connect to the container with the redis-cli.


```console
docker ps  # grab the new container id
docker inspect <container_id>    # grab the ipaddress of the container
redis-cli -h <ipaddress> -p 6379
redis 10.0.3.32:6379> set docker awesome
OK
redis 10.0.3.32:6379> get docker
"awesome"
redis 10.0.3.32:6379> exit
```

# Test 2
Connect to the host os with the redis-cli.


```console
docker ps  # grab the new container id
docker port <container_id> 6379  # grab the external port
ifconfig   # grab the host ip address
redis-cli -h <host ipaddress> -p <external port>
redis 192.168.0.1:49153> set docker awesome
OK
redis 192.168.0.1:49153> get docker
"awesome"
redis 192.168.0.1:49153> exit
```



# Install Docker on a Jenkins server
 
 Install and configure the user right to be able to access it.
 In particular, add the user jenkins to the group docker
 ```console
sudo yum -y install docker
sudo systemctl start docker
sudo systemctl enable docker
sudo groupadd docker
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
sudo systemctl restart docker
```
 
 
