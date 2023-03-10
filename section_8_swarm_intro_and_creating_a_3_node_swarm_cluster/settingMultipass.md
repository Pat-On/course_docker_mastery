# Installing Multipass and setting up Swarm

## Starting point

Links:

- https://multipass.run/
- https://www.pscp.tv/BretFisher/1mrGmQvNEWBGy

Planned Steps

- create 3 virtual machines
- install docker
- init swarm
- connect nodes to each other
- validate if it is working (simple nginx server?)

`multipass ls`

`multipass launch -n <name>`

Once you have Multipass VM's created,
then install docker and/or kubernetes inside them

- `multipass shell <name>` gets you into the VM shell,
- `multipass mount` can connect a host directory into the VM, and
- `multipass transfer` can copy files in.

- `multipass shell swarm1`
- `multipass info swarm1`

## installing docker

`multipass launch -n swarm1 --cloud-init .\docker.yaml`

`sudo docker swarm init`
`docker swarm join --token SWMTKN-1-04o14jclosy6ja6pv22hvrih2e4c2x0qd71t6e33333ndmzkm4-3ayukepmbyslq9ev9eh7xxh72 172.22.249.3:2377`

` docker swarm join --token SWMTKN-1-61eimt2cocjsh1cdn9qnuurb08rprmk0yfou3znhefdex8ihtoq-a0u413ugb3g632pk9bg4j7urc 172.22.255.223:2377`

`sudo docker node ls`

```
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
4rdmcz45zjsvegv36pk8d9qku     swarm1     Ready     Active                          20.10.17
cdbxall9e9u90k67xg12h8j14 *   swarm2     Ready     Active         Leader           20.10.17
q5m1ureckxuznn3e8noos4qmi     swarm3     Ready     Active                          20.10.17
```

TEST
`sudo docker service create --name nginx --publish 80:80 --replicas 3 nginx`

`sudo docker service scale z5dzysjrti4s=3`

## changing the role of the nodes

`docker node update --role manager <node_name>`

`docker swarm join-token manager`

`docker swarm join --token SWMTKN-1-61eimt2cocjsh1cdn9qnuurb08rp8ihtoq-3hva52teu7g59hg06x54d37f4 172.22.255.223:2377`

`sudo docker service ps z5dzysjrti4s`

```
ID             NAME      IMAGE          NODE      DESIRED STATE   CURRENT STATE            ERROR     PORTS
yriyrrd1cjit   nginx.1   nginx:latest   swarm2    Running         Running 10 minutes ago
a8ym8wdooi8f   nginx.2   nginx:latest   swarm3    Running         Running 10 minutes ago
dtyct82oil08   nginx.3   nginx:latest   swarm1    Running         Running 10 minutes ago
```
