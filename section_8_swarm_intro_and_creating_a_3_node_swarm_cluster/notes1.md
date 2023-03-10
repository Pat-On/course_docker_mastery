# Containers Everywhere = New Problems

- How do we automate container lifecycle?
- How can we easily scale out in up down?
- How can we ensure our containers are re-created if they fail?
- How can we replace containers without downtime (blue/green deploy)?
- How can we control/track where containers get started?
- How can we create cross-node virtual networks?
- How can we ensure only trusted servers run our containers?
- How can we store secrets, keys, passwords and get them to the right container (and only that container)?

# Swarm Mode: Built-in Orchestration

- Swarm Mode is a clustering solution built inside Docker
- Not related to Swarm 'classic' for pre-1.12 versions
- Added in 1.12 (summer 2016) via SwarmKit toolkit
- Enhanced in 1.13 (January 2017) via Stacks and Secrets
  - docker swarm
  - docker node
  - docker service
  - docker stack
  - docker secret

# 70 Create Your First Service and Scale It Locally

`docker info` - to find if we have swarm active
`docker swarm init` - initialization of the swarm

## docker swarm init: What Just Happened?

- Lots of PKI and security automation
  - Root Signing Certificate created for our Swarm
  - Certificate is issued for first Manager node
  - Join tokens are created
- Raft database created to store root CA, configs and secrets
  - Encrypted by default on disk (1.13+)
  - No need for another key/value system to hold orchestration/secrets
  - Replicates logs amongst Managers via mutual TLS in 'control plane'
  - Raft is protocol that ensure consistency across multiple nodes!

$ `docker node ls`

```
ID                            HOSTNAME         STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
9sb6x146kbyaemvag2k6mj9or *   docker-desktop   Ready     Active         Leader           20.10.23
```

$ `docker swarm --help`

```
Usage:  docker swarm COMMAND

Manage Swarm

Commands:
  ca          Display and rotate the root CA
  init        Initialize a swarm
  join        Join a swarm as a node and/or manager
  join-token  Manage join tokens
  leave       Leave the swarm
  unlock      Unlock swarm
  unlock-key  Manage the unlock key
  update      Update the swarm

Run 'docker swarm COMMAND --help' for more information on a command.
```

`pet vs cattle` :>

`docker service create alpine ping 8.8.8.8` - 8.8.8.8 is google dns service

$ `docker service ls    `

```
ID             NAME             MODE         REPLICAS   IMAGE           PORTS
nlfmbmcymoef   fervent_payne    replicated   1/1        alpine:latest
5omt4b0bewcv   modest_taussig   replicated   1/1        alpine:latest
```

`$ docker service ps 5omt4b0bewcv`

```
ID             NAME               IMAGE           NODE             DESIRED STATE   CURRENT STATE                ERROR     PORTS
9jzyi8a8tl1v   modest_taussig.1   alpine:latest   docker-desktop   Running         Running about a minute ago
```

`$ docker container ls`

```
CONTAINER ID   IMAGE           COMMAND          CREATED         STATUS         PORTS     NAMES
917b51b36b61   alpine:latest   "ping 8.8.8.8"   2 minutes ago   Up 2 minutes             modest_taussig.1.9jzyi8a8tl1v1j3cvkllrpvb3
c28e4a9b208a   alpine:latest   "ping 8.8.8.8"   3 minutes ago   Up 3 minutes             fervent_payne.1.7ju7rmqoubro8a8uwik9jam3r
```

## scaling up the service

`docker service update <id> --replicas 3

`$ docker service update 5omt4b0bewcv --replicas 3`

```
5omt4b0bewcv
overall progress: 3 out of 3 tasks
1/3: running   [==================================================>]
2/3: running   [==================================================>]
3/3: running   [==================================================>]
verify: Service converged
```

## `docker update`

` $ docker update --help`

```
Usage:  docker update [OPTIONS] CONTAINER [CONTAINER...]

Update configuration of one or more containers

Options:
      --blkio-weight uint16        Block IO (relative weight), between 10
                                   and 1000, or 0 to disable (default 0)
      --cpu-period int             Limit CPU CFS (Completely Fair
                                   Scheduler) period
      --cpu-quota int              Limit CPU CFS (Completely Fair
                                   Scheduler) quota
      --cpu-rt-period int          Limit the CPU real-time period in
                                   microseconds
      --cpu-rt-runtime int         Limit the CPU real-time runtime in
                                   microseconds
  -c, --cpu-shares int             CPU shares (relative weight)
      --cpus decimal               Number of CPUs
      --cpuset-cpus string         CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string         MEMs in which to allow execution (0-3, 0,1)
      --kernel-memory bytes        Kernel memory limit
  -m, --memory bytes               Memory limit
      --memory-reservation bytes   Memory soft limit
      --memory-swap bytes          Swap limit equal to memory plus swap:
                                   '-1' to enable unlimited swap
      --pids-limit int             Tune container pids limit (set -1 for
                                   unlimited)
      --restart string             Restart policy to apply when a
                                   container exits
```

`$ docker service update --help`

```
Usage:  docker service update [OPTIONS] SERVICE

Update a service

Options:
      --args command                       Service command args
      --cap-add list                       Add Linux capabilities
      --cap-drop list                      Drop Linux capabilities
      --config-add config                  Add or update a config file on
                                           a service
      --config-rm list                     Remove a configuration file
      --constraint-add list                Add or update a placement
                                           constraint
      --constraint-rm list                 Remove a constraint
      --container-label-add list           Add or update a container label
      --container-label-rm list            Remove a container label by its key
      --credential-spec credential-spec    Credential spec for managed
                                           service account (Windows only)
........
                                           ("start-first"|"stop-first")
      --update-parallelism uint            Maximum number of tasks
                                           updated simultaneously (0 to
                                           update all at once)
  -u, --user string                        Username or UID (format:
                                           <name|uid>[:<group|gid>])
      --with-registry-auth                 Send registry authentication
                                           details to swarm agents
  -w, --workdir string                     Working directory inside the
                                           container
```

### `docker container rm -f <name>.1.<ID>`

`$ docker container list`

```
CONTAINER ID   IMAGE           COMMAND          CREATED          STATUS          PORTS     NAMES
58d8e64ac626   alpine:latest   "ping 8.8.8.8"   4 minutes ago    Up 4 minutes              modest_taussig.3.lhfwg2rfkhehe889hs4sabxvz
82e24bc7312a   alpine:latest   "ping 8.8.8.8"   4 minutes ago    Up 4 minutes              modest_taussig.2.j1rl2nikm2e6r8h33zymbu9nf
917b51b36b61   alpine:latest   "ping 8.8.8.8"   10 minutes ago   Up 10 minutes             modest_taussig.1.9jzyi8a8tl1v1j3cvkllrpvb3
c28e4a9b208a   alpine:latest   "ping 8.8.8.8"   11 minutes ago   Up 11 minutes             fervent_payne.1.7ju7rmqoubro8a8uwik9jam3r
```

`$ docker container rm -f modest_taussig.1.n4tl0juj5gniyni3c2jp5h0f2`

```
modest_taussig.1.n4tl0juj5gniyni3c2jp5h0f2
```

`$ docker service ls`

```
ID             NAME             MODE         REPLICAS   IMAGE           PORTS
nlfmbmcymoef   fervent_payne    replicated   1/1        alpine:latest
5omt4b0bewcv   modest_taussig   replicated   2/3        alpine:latest
```

`$ docker service ps modest_taussig`

```
ID             NAME                   IMAGE           NODE             DESIRED STATE   CURRENT STATE           ERROR                         PORTS
jeo150m18yyk   modest_taussig.1       alpine:latest   docker-desktop   Running         Running 2 seconds ago
n4tl0juj5gni    \_ modest_taussig.1   alpine:latest   docker-desktop   Shutdown        Failed 7 seconds ago    "task: non-zero exit (137)"
9jzyi8a8tl1v    \_ modest_taussig.1   alpine:latest   docker-desktop   Shutdown        Failed 2 minutes ago    "task: non-zero exit (137)"
j1rl2nikm2e6   modest_taussig.2       alpine:latest   docker-desktop   Running         Running 8 minutes ago
lhfwg2rfkheh   modest_taussig.3       alpine:latest   docker-desktop   Running         Running 8 minutes ago
```

## removing service from swarm

`$ docker service rm modest_taussig`

```
modest_taussig
```

`$ docker container ls`

```
CONTAINER ID   IMAGE           COMMAND          CREATED          STATUS          PORTS     NAMES
c28e4a9b208a   alpine:latest   "ping 8.8.8.8"   17 minutes ago   Up 17 minutes             fervent_payne.1.7ju7rmqoubro8a8uwik9jam3r
```
