# Docker

Why do we need Docker?
The short list of benefits includes:

- faster development process
- handy application encapsulation
- the same behaviour on local machine / dev / staging / production servers
- easy and clear monitoring
- easy to scale


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


# Install docker
```console
sudo yum -y install docker
sudo groupadd docker
sudo usermod -aG docker user
sudo systemctl enable --now docker
```

# Check docker is running
```console
docker pull docker.io/hello-world
docker images
docker run hello-world
docker ps -a
```

# Create DockerFile and save it on the Hub
```console
vi Dockerfile
docker login
```

