# Docker Compose

Docker Compose is an orchestration framework that handles the building and running of multiple services (via separate containers) using a simple .yml file. 
It uses to link services together running in different containers.

```console
pip install docker-compose
```

Now let’s get web application up and running along with Redis:
```console
web:
    build: web
    volumes:
        - web:/code
    ports:
        - "80:5000"
    links:
        - redis
    command: python app.py
redis:
    image: redis:2.8.19
    ports:
        - "6379:6379"
```
Here we add the services that make up our stack:

1. web: First, we build the image from the “web” directory and then mount that directory to the “code” directory within the Docker container. The Flask app is ran via the python app.py command. This exposes port 5000 on the container, which is forwarded to port 80 on the host environment.
2. redis: Next, the Redis service is built from the Docker Hub “Redis” image. Port 6379 is exposed and forwarded.


```console
version: '1.0'
services:
  java-web-frontend:
    image: TerryV8/click-count
    container_name: java-web-frontend
    hostname: java-web-frontend
    env_file: java-web-frontend.env
    restart: always
    ports:
      - "8080:8080"
    volumes:
      - .:/code
    depends_on:
      - redis

  redis:
    image: redis
#    volumes:
#      - /redis_data:/data
```

```console
version: '1.0'
services:
  java-web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
    depends_on:
      - redis

  redis:
    image: redis
#    volumes:
#      - /redis_data:/data
```



```console
version: '1.0'  
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

# Build and Run
With one simple command we can build the image and run the container.
Start the application from the current directory:
```console
docker-compose up --build
```

If you have localhost access to your host (i.e., you do not use a remote solution to deploy Docker), point your browser to http://0.0.0.0:5000, http://127.0.0.1:5000, or http://localhost:5000. On a Mac, you need to use docker-machine ip MACHINE_VM to get your Docker host’s IP address (then use that address like http://MACHINE_IP:5000 to access your web page). If you do use a remote host, simply use that IP address and append :5000 to the end.
