# Docker Networks: Concepts for Private and Public Communication in Containers

- `docker container run -p `
- for local dev/testing, networks usually 'just work'
- quick port check with `docker container port <container>`
- Learn concepts of Docker Networking
- Understand how network packets move around Docker

---

### Docker Networks Defaults

- Each container connected to a private virtual network 'bridge'
- Each virtual network routes through NAT firewall on host IP
- All containers on a virtual network can talk to each other without -p
- best practice is to create a new virtual network for each app: - network 'my_web_app' for mysql and php/apache containers - network 'my_api' for mongo and nodejs containers

### Docker Networks Cont.

- "Batteries Included, But Removable" - Defaults work well in many cases, but easy to swap out parts to customize it
- Make a new virtual networks
- Attach containers to more than one virtual network (or more)
- Skip virtual networks and use host IP (`--net=host`)
- Use different Docker network drivers to gain new abilities

### Command Line

`docker container run -p 80:80 --name webhost -d nginx`
`-p (-publish)` - publishing ports are always in `HOST:CONTAINER` format

`docker container port webhost`
output:: `80/tcp -> 0.0.0.0:80`

`docker container inspect --format `
`docker container inspect --format "{{.NetworkSettings.IPAddress}}" webhost`
Note: if using Windows, you may need to use double quotes rather than single quotes for --format

`--format` - A common option for formatting the output of commands using 'Go Templates'.

### Traffic Flow and Firewalls

#### How Docker networks move packets in and out

```
        container1 --------> bridge/docker0-------->firewall-------->Ethernet------>net
                            virtual network                          Interface

                                                                    HOST:CONTAINER
        -p 80:80                                                       80:80
                                                                    It is define to forward the req to and back
```

Important: That is why you can not use the same port on your host to listen to req/res

---


