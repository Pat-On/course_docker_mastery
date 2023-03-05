# Assignment: Using Containers for CLI

- Use different Linux distro containers to check `curl` cli tool version
- Use two different terminal windows to start bash in both `centos:7` and `ubuntu:14.04`, using `-it`
- Learn `docker container --rm` option so you can save cleanup
- Ensure `curl` is installed and on latest version for that distro - ubuntu: `apt-get update && apt-get install curl` - centos: `yum update curl`
- Check `curl --version`

---

`docker container run --rm -it centos:7 bash` - it will remove machine after all
`docker container run --rm -it ubuntu:14.04 bash`

# Assignment: DNS Round Robin Test

More about it: https://en.wikipedia.org/wiki/Round-robin_DNS

- Ever since Docker Engine 1.11, We can have multiple containers
  on a created network respond to the same DNS address
- Create a new virtual network (default bridge driver)
- Create two containers from elasticsearch:2 image
- Research and use `--network-alias search` when creating them to give them additional DNS name to respond to
- Run `alpine nslookup search` with `--net` to see the two containers list for the same DNS name
- Run `centos curl -s search:9200` with `--net` multiple times until you see both "name" fields show

Changes to assignment:

- replace centos with alpine

- `docker run -e "discovery.type=single-node" -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" -e "xpack.security.enabled=false" --network <your_network> -d --network-alias search elasticsearch:8.4.3`

Then:

```
docker run --rm -it --network <your_network> alpine
# use nslookup to see both container IPs for the same DNS A record
nslookup search
# install curl and run it
apk add curl
curl search
curl search
```

option 2:
replace elastic search with `bretfisher/httpenv` - port 8888

`docker container run -d --net dude --net-alias search elasticsearch:2`

`--net-alias` and `--network-alias` - both work!

`docker container run --rm --net dude centos curl -s search:9200`
