# Section 4

`docker version` - check your versions and that docker is working

`docker info` - shows most configuration values for the engine

`docker` - shows some commands

## Docker command format

new 'management commands' format:
`docker <command> <sub-command> (options)`

old way(still works):
`docker <command>(options)`

## Starting a Nginx Web Server

- image vs. container
- run/stop/remove containers
- check containers logs and processes

[note: If You are using Docker Toolbox, type in the ip address 192.168.99.100]

### Image vs. Container

- An image is the application we want to run
- a container is an instance of that image running as a process
- you can have many containers running of the same image
- in this lecture our image will be the nginx web server
- Docker's default image "registry" is called Docker Hub (hub.docker.com)

`docker container run `- starts a bew container from an image (old way - docker run)

`docker container run --publish 80:80 nginx`

1. Downloaded image 'nginx' from Docker Hub
2. Started a new container from that image
3. Opened port 80 on the host IP
4. Routes that traffic to the container IP, port 80

Note: You will get a bind error if the lefty number (host port )
is being used by anything else, even another container.
You can use any port you want on the left, like 8080:80
or 8888:80, then use localhost:8888 when testing.

Interesting:
ctr-c - sen d a stop signal to the process when running in foreground
turns quit ctr-c does not work the same way on Windows.
It exists the foreground but leaves container running in background

docker container run --publish 80:80 --detach nginx

`docker container stop (container id)` - stop the container process but does not remove it

#### run vs start

'docker container run' always starts a new container
use docker container start to start an existing stopped one

`docker ls -a`

`docker container run --publish 80:80 --detach --name webhost  nginx`

`docker container logs webhost`

`docker container top webhosT`

### Removing all containers

`docker container rm (container_id)` - remove delete one or more containers

`docker container rm -f (container_id)` - with forcing flag
