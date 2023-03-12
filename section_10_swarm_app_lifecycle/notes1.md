# 85 Using Secrets With Local Docker Compose

test it inside secret-sample-2 folder

`docker compose up -d`

`docker compose exec psql cat /run/secrets/psql_user`

it works only with file based secrets not with the external

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
