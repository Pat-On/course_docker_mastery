# 79 Swarm Stacks and Production Grade Compose

# Stacks

- in 1.13 Docker adds a new layer of abstraction to Swarm called Stacks
- Stacks accept Compose files as their declarative definition for services, networks and volumes
- we use `docker stack deploy` rather then docker service create
- Stacks manages all those objects for us, including overlay network per stack. Adds stack name to start of their name
- New `deploy:` key in Compose file. Can not do `build:`
- Compose now ignores `deploy:`, Swarm ignores `build:`
- `docker compose` cli not needed on Swarm server

```
                    Services

             service
                                            task        container
                    3 nginx replicas    -> nginx.1   nginx:latest
                    swarm manager       -> nginx.2   nginx:latest              volumes
                                        -> nginx.3   nginx:latest

 Stack:             3 nginx replicas    -> nginx.1   nginx:latest
                    swarm manager       -> nginx.2   nginx:latest               secrets
                                        -> nginx.3   nginx:latest

                    3 nginx replicas    -> nginx.1   nginx:latest
                    swarm manager       -> nginx.2   nginx:latest              overlay networks
                                        -> nginx.3   nginx:latest
```

It runs only one swarm!

compose file + `sudo docker stack deploy -c example-voting-app-stack.yml voteapp` NICE!

This command very quickly created scheduled tasks and slowly started to build app!

`docker stack`

```

Usage:  docker stack COMMAND

Manage Swarm stacks

Commands:
  config      Outputs the final config file, after doing merges and interpolations
  deploy      Deploy a new stack or update an existing stack
  ls          List stacks
  ps          List the tasks in the stack
  rm          Remove one or more stacks
  services    List the services in the stack

Run 'docker stack COMMAND --help' for more information on a command.
```

# to check how entire app is working you need three commands:

## `sudo docker stack services voteapp`

```
ID             NAME                 MODE         REPLICAS   IMAGE                                       PORTS
wbarnbskdm6z   voteapp_db           replicated   1/1        postgres:9.4
z9dxz477y080   voteapp_redis        replicated   1/1        redis:alpine
pu43czw1r2ev   voteapp_result       replicated   1/1        bretfisher/examplevotingapp_result:latest   *:5001->80/tcp
qysdg7uvot7g   voteapp_visualizer   replicated   1/1        bretfisher/visualizer:latest                *:8080->8080/tcp
tm2w99k1dsyb   voteapp_vote         replicated   2/2        bretfisher/examplevotingapp_vote:latest     *:5000->80/tcp
bgw9f1v5z8f5   voteapp_worker       replicated   1/1        bretfisher/examplevotingapp_worker:latest
```

## `sudo docker stack ps voteapp`

```
ID             NAME                   IMAGE                                       NODE      DESIRED STATE   CURRENT STATE
  ERROR                       PORTS
5tyd445yyrxt   voteapp_db.1           postgres:9.4                                swarm1    Running         Running 4 minutes ago

jjmsf9e43vxj   voteapp_redis.1        redis:alpine                                swarm2    Running         Running 4 minutes ago

k2j8czmvzgcj   voteapp_result.1       bretfisher/examplevotingapp_result:latest   swarm1    Running         Running 4 minutes ago

n691gdy37alf   voteapp_visualizer.1   bretfisher/visualizer:latest                swarm3    Running         Running 3 minutes ago

errpemrof6z7   voteapp_vote.1         bretfisher/examplevotingapp_vote:latest     swarm2    Running         Running 4 minutes ago

ofsvmfhcd38b   voteapp_vote.2         bretfisher/examplevotingapp_vote:latest     swarm3    Running         Running 4 minutes ago

1t3f784ix2gu   voteapp_worker.1       bretfisher/examplevotingapp_worker:latest   swarm1    Running         Running 4 minutes ago

vhk5mw70f4ws    \_ voteapp_worker.1   bretfisher/examplevotingapp_worker:latest   swarm1    Shutdown        Failed 4 minutes ago
  "task: non-zero exit (1)"
```

`sudo docker network ls`

```
NETWORK ID     NAME               DRIVER    SCOPE
xpeyp97dolnn   backend            overlay   swarm
20107932f3ef   bridge             bridge    local
f1fc0a66c492   docker_gwbridge    bridge    local
bmme1ubf2idu   frontend           overlay   swarm
0b17f3a665e5   host               host      local
05i2r1m1rubl   ingress            overlay   swarm
vfb3wvlxi39v   mydrupal           overlay   swarm
862fef3f9dff   none               null      local
xkpwoyusingj   voteapp_backend    overlay   swarm
l4bj377c0afy   voteapp_frontend   overlay   swarm
```

IMPORTANT TO REMEMBER:

WHEN YOU ARE RUNNING APPS BASED ON YAML/COMPOSE FILES
YOU ARE NOT CHANGING VALUE BY COMMANDS IN INFRASTRUCTURE BUT
EVERY SINGLE OPERATION SHOULD BE PROCEEDED BASED ON YAML/COMPOSE FILE CHANGES
