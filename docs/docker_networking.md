# Link network Docker connection between back-end & front-end

Running only:
```console
docker run --name web-app -p 8080:8080 -d terryv8/web-app
```
without using the --link option, won't allow the back-end Redis container and front-end web-app container to coomunicate each other


So To have a runnable image with -p to specify the port mapping between the port of my host 
and the port inside my container, -d for a detached mode and allowing network communication with redis container is:

docker run --name web-app -p 8080:8080 -d --link=redis terryv8/web-app
