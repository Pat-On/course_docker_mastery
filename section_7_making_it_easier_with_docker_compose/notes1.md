# Docker Compose and the docker-compose.yml File

## Docker Compose

- Why: configure relationships between containers
- Why: save our docker container run settings in easy-to-read file
- Why: create one-liner developer environment startups
- Comprised of 2 separate but related things
- 1. Yaml-formatted file that describes our solution options for:
  - containers
  - networks
  - volumes
- 2. A CLI tool `docker-compose` used for local dev/test automation with those YAML files

## docker-compose.yml

- Compose YAML format has it's own versions: 1, 2, 2.1, 3, 3.1
- YAML file can be used with `docker-compose` command for local docker automation or..
- with `docker` directly in production with Swarm (a of v1.13)
- `docker-compose --help`
- `docker-compose.yml` is default filename, but any can be used with `docker-compose -f`

```YAML
# version isn't needed as of 2020 for docker compose CLI.
# All 2.x and 3.x features supported
# Docker Swarm still needs a 3.x version
# version: '3.9'

services:  # containers. same as docker run
  servicename: # a friendly name. this is also DNS name inside network (containers)
    image: # Optional if you use build:
    command: # Optional, replace the default CMD specified by the image
    environment: # Optional, same as -e in docker run
    volumes: # Optional, same as -v in docker run
  servicename2:

volumes: # Optional, same as docker volume create
# docker volume

networks: # Optional, same as docker network create
# docker network

```

## Interesting links:

https://yaml.org/refcard.html
https://docs.docker.com/compose/compose-file/compose-versioning/
https://github.com/docker/compose/releases
https://yaml.org/
https://nodeca.github.io/js-yaml/

---

## Compose V2

In 2022, Docker announced the General Availability of Docker Compose V2.

It supports all the same commands taught in this course and is meant to be fully backward compatible. It's auto-installed by Docker Desktop.

All you need to do is simply remove the dash from your Docker Compose commands:

docker-compose up becomes docker compose up, etc.

Behind the scenes, Docker has rebuilt the old docker-compose Python binary with go, the same language as the Docker CLI, and added Compose V2 as a CLI plugin rather than a separate command. It's now faster and more stable, and should "just work" as a drop-in replacement for the V1 docker-compose CLI.

So anywhere in this course that I type docker-compose, just replace that with docker compose!
