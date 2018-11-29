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







# Install docker
```console
sudo yum -y install docker
sudo groupadd docker
sudo usermod -aG docker user
sudo systemctl start docker
sudo systemctl enable docker
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
docker build -t <docker username>/<image-name>
```

Once you build the image, you can create and run a container instance:
```console
docker run -d <docker username>/<image-name>
```

To see running containers:
```console
docker ps
```


```console
vi Dockerfile
docker login
```

# with dockercompose
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
      - "5000:5000"
  redis:
    image: redis:3.2-alpine
    volumes:
      - redis_data:/data
volumes:  
  redis_data:
  
  
Alpine images

A lot of Docker images (versions of images) are created on top of Alpine Linux – this is a lightweight distro that allows you to reduce the overall size of Docker images.

I recommend that you use images based on Alpine for third-party services, such as Redis, Postgres, etc. For your app images, use images based on buildpack – it will be easy to debug inside the container, and you’ll have a lot of pre-installed system-wide requirements.

Only you can decide which base image to use, but you can get the maximum benefit by using one basic image for all images, because in this case the cache will be used more effectively.
  
