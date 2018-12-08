
The basic command line to run the web-app using Docker with binding the container port to the specific port, 
and allowing network communication with redis container is:

docker run --name web-app -p 8080:8080 -d --link=redis terryv8/web-app



