# 73 Creating a 3-Node Swarm Cluster

- A. play-with-docker.com
  - Only needs a browser, but resets after 4 hours
- B. docker-machine + VirtualBox
  - Free and runs locally, but requires a machine with 8GB memory
- C. Digital Ocean + Docker install
  - Most like a production setup, but cost $5-10/node/month while learning
- D. Roll your own
  - docker-machine can provision machines for Amazon, Azure, DO, Google, etc.
  - install docker anywhere with get.docker.com


## swarm commands to start
after installing the docker... 
`docker swarm init` <- you need to advertise since this system has multiple addresses on interface

so:

`docker swarm init --advertise-addr 104-236-114-90`

output from this command:
`docker swarm join --token <LONG_TOKEN> 104.236.114.90:2377`

`docker node update --role manager node2`


`docker swarm join-token manager`  - you can get this token whenever you want. you can change them, rotate etc.
