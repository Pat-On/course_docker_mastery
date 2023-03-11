# 75 Scaling out with overlay networking

- just choose `--driver overlay` when creating network
- for container-to-container traffic inside a single Swarm
- Optional IPSec (AES) encryption on network creation
- Each service can be connected to multiple networks
  - eg front-end or back-end

drupal:9
postgres:14

1.  Create network
    `docker network create --driver overlay mydrupal`
    `docker network ls`

        ```
        NETWORK ID     NAME              DRIVER    SCOPE
        22e01ac82c95   bridge            bridge    local
        8b4398829302   docker_gwbridge   bridge    local
        c02b877f6d99   host              host      local
        05i2r1m1rubl   ingress           overlay   swarm
        vfb3wvlxi39v   mydrupal          overlay   swarm
        ae207fd14056   none              null      local
        ```

2.  Create a first service
    `docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgres:14`

    then to validate:
    `docker service ls`
    `docker service ps psql`
    `docker container logs psql.1.blabla`

3.  Create a second service
    `docker service create --name drupal --network mydrupal -p 80:80 drupal:9`

# 76 Scaling Out with Routing Mesh

### Routing Mesh

- Routes ingress (incoming) packets for a Service to proper Task
- Spans all nodes in Swarm
- Uses IPVS from Linux Kernel (https://en.wikipedia.org/wiki/IP_Virtual_Server)
- Load balances Swarm Services across their Tasks
- Two ways in this works
- Container-to-container in a Overlay network (uses VIP)
- External traffic incoming to published ports (all nodes listen)
  - You do not care on what node container lives! Nice!

`docker service create --name search --replicas 3 -p 9200:9200 elasticsearch:2`

then: `curl localhost:9200`

### Routing Mesh Cont.

- (in version 17.03) This is stateless load balancing
- This LB is at OSI Layer 3 (TCP), not Layer 4 (DNS)
- Both limitation can be overcome with:
  - Nginx or HAProxy LB proxy, or
  - Docker Enterprise Edition, which comes with built-in L4 web proxy

#### Networks in the Swarm

bridge - this network driver only allows containers to talk on a single node, and not across nodes
overlay - this network driver is used for container communication across a swarm

additional reading: https://docs.docker.com/engine/swarm/ingress/
