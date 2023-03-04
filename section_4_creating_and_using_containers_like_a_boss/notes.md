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

---

## What happens in `docker container run`

1. Looks for that image locally in image cache, does not find anything
2. Then looks in remote image repository (default to Docker Hub)
3. Downloads the latest version (nginx:latest by default)
4. Creates new container based on that image and prepares to start
5. Gives it a virtual IP on a private network inside docker engine
6. Opens up port 80 on host and forwards to port 80 in container
7. Starts container by using the CMD in the image Dockerfile

Example:
`docker container run --publish 8080:80 --name webhost -d nginx:1.11 nginx -T`

---

## Container vs VM

### it is just a process

Containers are not Mini-VM's

- There are just processes
- Limited to what resources they can access
- Exit when process stops

(it is restricted process in our operating system)

`docker run --name mongo -d mongo`

`docker top` - list running processes inb specific container

`docker top mongo`

(The ps command I show here looks at the host, but Mac/Win Docker is running in a mini-VM,
so if you want to run ps on those, you will need to connect to the Docker VM itself)

`ps aux` - show me all running processes

Resources from the lecture:

https://www.bretfisher.com/getting-a-shell-in-the-docker-for-windows-vm/
https://www.bretfisher.com/docker-for-mac-commands-for-getting-into-local-docker-vm/
https://www.youtube.com/watch?v=sK5i-N34im8&list=PLBmVKD7o3L8v7Kl_XXh3KaJl9Qw2lyuFl
https://github.com/mikegcoleman/docker101/blob/master/Docker_eBook_Jan_2017.pdf

`ps aux | grep mongo`

---

## Assignment: manage Multiple Containers

- docs.docker.com and `--help` are your friend xD
- run a nginx, a mysql and a httpd (apache) server
- run all of them `--detach` (-d), name them with --name
- nginx should listen on 80:80, httpd on 8080:80, mysql on 3306:3306
- when running mysql, use the --env option (or -e) to pass in `MYSQL_RANDOM_ROOT_PASSWORD=yes`
  use `docker container logs` on mysql to find the random password it created on startup
- clean it all up with `docker container stop` and `docker container rm`
  (both can accept multiple names or ID's)

---

## Assignment: solution

`docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql`

`docker container logs db`

`2023-03-04 10:51:53+00:00 [Note] [Entrypoint]: GENERATED ROOT PASSWORD: KpYD1OGw70KRIWynnyS5HJuURhsgCX0n`

`docker container run -d --name webserver -p 8080:80 httpd`

`docker ps` | `docker container ls`

```
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS
              NAMES
b741bd598bb9   httpd     "httpd-foreground"       12 seconds ago   Up 5 seconds   0.0.0.0:8080->80/tcp                webserver
23b121bc0da6   mysql     "docker-entrypoint.s…"   3 minutes ago    Up 3 minutes   0.0.0.0:3306->3306/tcp, 33060/tcp   db
```

`docker container run -d --name proxy -p 80:80 nginx`

`docker ps` | `docker container ls`

```
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                               NAMES
9eef75a0ac0c   nginx     "/docker-entrypoint.…"   3 seconds ago        Up 2 seconds        0.0.0.0:80->80/tcp                  proxy
b741bd598bb9   httpd     "httpd-foreground"       About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp                webserver
23b121bc0da6   mysql     "docker-entrypoint.s…"   4 minutes ago        Up 3 minutes        0.0.0.0:3306->3306/tcp, 33060/tcp   db
```

to test:
`curl localhost`

`curl localhost:8080`

`docker container rm -f  proxy webserver db`
