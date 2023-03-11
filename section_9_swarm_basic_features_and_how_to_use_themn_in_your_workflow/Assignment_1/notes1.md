# 77 Assignment: Create A Multi-Service Multi-Node Web App

- Using Docker's Distributed Voting App
- use `swarm-app-1` directory in our course repo for requirements
- 1 volume, 2 networks, and 5 services needed
- create the commands needed, spin up services, and test app
- Everything is using Docker Hub images, so no data needed on Swarm
- Like many computer things, this is 1/2 art form and 1/2 science

# Answer

`sudo docker ps -a` - show all containers

### Networks

`docker network create -d overlay frontend`
`docker network create -d overlay backend` creating the network connecting different nodes
so you can spam containers across nodes

### FRONTEND

Now we are going to create vote and place it inside the frontend network
command:
`docker service create --name vote -p 80:80 --network frontend --replicas 2 bretfisher/examplevotingapp_vote`

creating redis
`docker service create --name redis --network frontend --replicas 1 redis:3.2`

### BACKEND

creating database - you do not need to specify if you only need one replica
`docker service create --name db --network backend -e POSTGRES_HOST_AUTH_METHOD=trust --mount type=volume,source=db-data,target=/var/lib/postgresql/data postgres:9.4`
We are using in swarm mount.

workers
`docker service create --name worker --network frontend --network backend bretfisher/examplevotingapp_worker`

results service
`docker service create --name result --network backend -p 5001:80 bretfisher/examplevotingapp_result`

### Verify

`sudo docker service ls`

```
ID             NAME      MODE         REPLICAS   IMAGE                                       PORTS
kcy86pmp1wqp   db        replicated   1/1        postgres:9.4
le1xwn5izdkd   redis     replicated   1/1        redis:3.2
scwye9442raa   result    replicated   1/1        bretfisher/examplevotingapp_result:latest   *:5001->80/tcp
3o4coc26q5zz   vote      replicated   2/2        bretfisher/examplevotingapp_vote:latest     *:80->80/tcp
jlabg8h0kzb0   worker    replicated   1/1        bretfisher/examplevotingapp_worker:latest
```

### Docker service logs (17.05) pull logs from many different nodes

`docker service logs worker`
