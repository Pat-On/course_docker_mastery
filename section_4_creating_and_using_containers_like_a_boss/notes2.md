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

# Docker Networks: CLI Management of Virtual Networks

- show networks `docker network ls`
- inspect a network `docker network inspect`
- create network `docker network create --driver`
- Attach a network to container `docker network connect`
- detach a network from container `docker network disconnect`

`docker network ls`

```
NETWORK ID     NAME      DRIVER    SCOPE
1c151c47b31f   bridge    bridge    local
eedc9c351027   host      host      local
28eaf1d45f04   none      null      local
```

`--network bridge` - default docker virtual network, which is NAT`ed behind the Host IP

```
docker network inspect bridge
```

```
 "Name": "bridge",
        "Id": "1c151c47b31f521817541bb97ae6ba6e9c0470d23970c39bfc36a3e53d60576a",
        "Created": "2023-03-05T06:31:18.927823446Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "a650048a2da5af70c7ea70821a12306c4520c97cf98d7247cf309f9a37d957af": {
                "Name": "webhost",
                "EndpointID": "ba91db4d62f6591d11f6e1021a065477164f5ef06e235bfb8b12b7edb14875d0",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
```

`--network host` - It gains performance by skipping virtual networks but sacrifices security of container model

`--network none` removes eth0 and only leaves you with localhost interface in container

`docker network create` - spawns a new virtual network for you to attach containers to
`docker network create my_app_network`

```
NETWORK ID     NAME             DRIVER    SCOPE
1c151c47b31f   bridge           bridge    local
eedc9c351027   host             host      local
9814622e74b2   my_app_network   bridge    local
28eaf1d45f04   none             null      local
```

`network driver` - Built-in or 3rd party extensions that give you virtual network features

` docker network create --help`

```

Usage:  docker network create [OPTIONS] NETWORK

Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by Network driver (default map[])
      --config-from string   The network from which to copy the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a network segment
```

`docker container run -d --name new_nginx --network my_app_network nginx`

```
        "Name": "my_app_network",
        "Id": "9814622e74b2e24c3b4260928f4754cd4b2ee5734b28c8fc19c4dd8588f30e48",
        "Created": "2023-03-05T06:58:50.441738536Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "c8acdc10866dcf991d53fcd61c875a31acc9cf06a53592cb1af22673c157642f": {
                "Name": "new_nginx",
                "EndpointID": "ba04370219f2a8953dab6086489bf9c691f2ef7727e1ab1de12cfa6750e6e16d",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
```

`docker network connect` - Dynamically creates a NIC in a container on an existing virtual network

`$ docker network connect my_app_network webhost`

```
  "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "1c151c47b31f521817541bb97ae6ba6e9c0470d23970c39bfc36a3e53d60576a",
                    "EndpointID": "ba91db4d62f6591d11f6e1021a065477164f5ef06e235bfb8b12b7edb14875d0",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                },
                "my_app_network": {
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "a650048a2da5"
                    ],
                    "NetworkID": "9814622e74b2e24c3b4260928f4754cd4b2ee5734b28c8fc19c4dd8588f30e48",
                    "EndpointID": "9db60e5cc3ac44ea1829ca05542c6c0ee963716ef02ecceabac60441771bda9a",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:03",
                    "DriverOpts": {}
                }
            }
```

`docker network disconnect` - Dynamically removes a NIC from a container on a specific virtual network

```
 "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "1c151c47b31f521817541bb97ae6ba6e9c0470d23970c39bfc36a3e53d60576a",
                    "EndpointID": "ba91db4d62f6591d11f6e1021a065477164f5ef06e235bfb8b12b7edb14875d0",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
```

## Docker Networks: Default Security

- Create your apps so frontend/backend sit on same Docker network
- Their inter-communication never leaves host
- All externally exposed ports closed by default
- You must manually expose via -p, which is better default security!\
- This gets even better later with Swarm and Overlay networks

---

# Docker Networks: DNS and How Containers Find Each Other

- Understand how DNS is the key to easy inter-container comms
- see how it works by default with custom networks
- learn how to use `--link` to enable DNS on default bridge network

!Important
Forget IS's
Static IP's and using IP's for talking to containers is an anti-pattern.
Do your best to avoid it!

`Docker DNS` - Docker daemon has a built-in DNS server that containers use by default
`Docker Default Names` - Docker default the hostname to the container's name, but you can also set aliases.

### Docker Networks: DNS

- Containers should not rely on IP's for inter-communication
- DNS for friendly names ios built-in if you use custom networks
