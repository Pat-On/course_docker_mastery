# 85 Using Secrets With Local Docker Compose

test it inside secret-sample-2 folder

`docker compose up -d`

`docker compose exec psql cat /run/secrets/psql_user`

it works only with file based secrets not with the external

---

# 86 Full App Lifecycle:

## DEV, Build and Deploy with a Single Compose Design

Links:
Use Compose in production: https://docs.docker.com/compose/production/
Share Compose configurations between files and projects: https://docs.docker.com/compose/extends/#multiple-compose-files

Full App Lifecycle with Compose

- Live the dream! xD
- Single set of Compose files for:
- Local `docker compose up` development environment
- remote `docker compose up` CI env
- Remote `docker stack deploy` production env
- NOTE: `docker compose -f a.yml -f b.yml config` mostly
- NOTE: compose `extends:` does not work yet in stack <-- 2023 is it?

## commands

### DEV

`sudo docker compose up`

inside folder with

- docker-compose.yml
- docker-compose.override.yml
- docker-compose.prod.yml
- docker-compose.test.yml
  is going to by default use override file to override the docker-compose file

### TEST

`docker compose -f docker-compose.yml -f docker-compose.test.yml up -d`
This command is going to take dockerfile and start to override them in order

1. file must be base file then next files.

in the results we will not have mounts in this build because we did not specify them in the test! nice!

### PROD

`docker compose -f docker-compose.yml -f docker-compose.prod.yml config > output.yml`

This command is going to combine this files into one - this file would be the file that we would use in the production

---

# 87 Service Updates: Changing Things in Flight

docker service update - https://docs.docker.com/engine/reference/commandline/service_update/

## Service Updates

- Provides rolling replacement of tasks/containers in a service
- Limits downtime (be careful with 'prevents' downtime)
  - persistent things that require persistent connections are going to be the thoughts to update
- Will replace containers for most changes
  - metadata label etc will not trigger it probably :>
- Has many, many cli options to control the update
- create options will usually change, adding -add or -rm to them
- Includes rollback and health check options
- also has scale & rollback subcommand for quicker access
  - `docker service scale web=4` and `docker service rollback web`
- A stack deploy, when pre-existing, will issue service updates!

## Swarm Update Examples

- Just update the image used to a newer version
  - `docker service update --image myapp:1.2.1 <servicename>`
- Adding an env variable and remove a port
  - `docker service update --env-add NODE_ENV=production --publish-rm 8080`
- Change number of replicas of two services
  - docker service scale web=8 api=6

## Swarm Updates in Stack Files

- Same command. Just edit the YAML file, then
  - `docker stack deploy -c file.yml <stackname>`

---

## Commands used during testing:

- `docker service create -p 8088:80 --name web nginx:1.13.7`
- `docker service scale web=5`
- `docker service update --image nginx:1.13.6 web`
- `docker service update --publish-rm 8088 --publish-add 9090:80`

Docker Tip:
FORCING UPDATE!
YOu can use it to rebalanced the nodes!

`docker service update --force web`

# 88 Health-checks in Dockerfile

Docker Health-checks

- HEALTHCHECK was added in 1.12
- supported in Dockerfile, Compose YAML, docker run and Swarm services
- Docker engine will `exec`'s the command in the container
  - (e.g. `curl localhost`)
- it expects `exit 0` (ok) or `exit 1` (Error)
- Three container states: starting (first 30 seconds), healthy (if 0), unhealthy (for example exit 1)
- much better than "is binary still running?"
- not a external monitoring replacement
  - it is more for docker to understand if the container is healthy - it is not advance monitoring tool

## Docker healthcheck container

- Healthcheck status shows up in `docker container ls`
- check last 5 healthchecks with `docker container inspect`
- docker run does nothing with healthchecks
- service will replace tasks if they fail healthcheck
- service updates wait for them before counting

## Healthcheck Docker Run Example

(existing docker container that has no implemented healthcheck)

```
docker run \
--health-cmd='curl -f localhost:9200/_cluster/health || false" \
--health-interval=5s \
--health-retries=3 \
--health-timeout=2s \
--health-start-period=15s \
elasticsearch:2
```

## Healthcheck Dockerfile Example

- Options for healthcheck command

  - --interval=DURATION (default: 30s)
  - --timeout=DURATION (default: 30s)
  - --start-period=DURATION (default: 0s) (17.09++)
  - --retries=N (default: 3)

- Basic command using default options

  - `HEALTHCHECK curl -f http://localhost/ || false`

- Custom options with the command

  ```
  HEALTHCHECK --timeout=2s --interval=3s --retries=3 \
  CMD curl -f http:localhost/ || exit 1
  ```

## Healthcheck in Nginx Dockerfile

static website running in Nginx, just test default URL

```
FROM nginx:1.13

HEALTHCHECK --interval=30s --timeout=3s \
 CMD curl -f http://localhost/ || exit 1
```

sudo docker container run --name p1 -e POSTGRES_PASSWORD=pass -d postgres
sudo docker container run --name p2 -e POSTGRES_PASSWORD=pass -d --health-cmd="pg_isready -U postgres || exit 1" postgres

` sudo docker container ls -a`

```
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS                            PORTS      NAMES
37123651d4bb   postgres   "docker-entrypoint.s…"   2 seconds ago    Up 2 seconds (health: starting)   5432/tcp   p8
66c3abf998bd   postgres   "docker-entrypoint.s…"   40 seconds ago   Up 40 seconds                     5432/tcp   p4
```
