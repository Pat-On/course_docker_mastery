# Container Lifetime & Persistent Data

#### Section Overview

- Defining the problem of persistent data
- Key concepts with containers: immutable, ephemeral
- Learning and using Bind Mounts
- Assignments - around how to preserve data and how to mount the code

#### Container Lifetime & Persistent Data

- Containers are usually immutable and ephemeral
- 'immutable infrastructure': only re-deploy containers, never change
- This is the ideal scenario, but what about databases, or unique data?
- Docker gives us features to ensure these "separation of concerns"
- This is known as "persistent data"
- Two ways: Volumes and Bind Mounts
- Volumes: make special location outside of containers UFS
- Bind Mounts: link container path to host path

Interesting links:
https://www.oreilly.com/radar/an-introduction-to-immutable-infrastructure/
https://12factor.net/
https://medium.com/@kelseyhightower/12-fractured-apps-1080c73d481c#.cjvkgw4b3
https://docs.docker.com/storage/

# Persistent Data: Data Volumes

- VOLUME command in Dockerfile

from mysql docker image:
`VOLUME /var/lib/mysql`

`docker pull mysql`

`docker image inspect mysql`

```
            "Volumes": {
                "/var/lib/mysql": {}
            },
```

`docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql`

`docker container inspect mysql`

```
        "Mounts": [
            {
                "Type": "volume",
                "Name": "e090c91765f711d003c1837ae41680c1f289851138fcf5d21faf57bfe29a2363",
                "Source": "/var/lib/docker/volumes/e090c91765f711d003c1837ae41680c1f289851138fcf5d21faf57bfe29a2363/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
```

Mounts - it is running container that is getting itself unique location and then it is mounted to the container.
Data lives on the host under the "Source"

`docker volume ls`

```
DRIVER    VOLUME NAME
local     e090c91765f711d003c1837ae41680c1f289851138fcf5d21faf57bfe29a2363
```

`docker volume inspect e090c91765f711d003c1837ae41680c1f289851138fcf5d21faf57bfe29a2363`

```
[
    {
        "CreatedAt": "2023-03-07T07:23:26Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/e090c91765f711d003c1837ae41680c1f289851138fcf5d21faf57bfe29a2363/_data",
        "Name": "e090c91765f711d003c1837ae41680c1f289851138fcf5d21faf57bfe29a2363",
        "Options": null,
        "Scope": "local"
    }
]
```

Note:
On the linux this data is going to be in your host
On the Max and Windows this data is going to be stored for example in the WSL in case of windows machine

### checking from volume perspective what is connected to it is possible

`named volumes` - friendly way to assign vols to containers

The same action like default - volume name is going to be just hash
`docker container run -d --name mysql_new -e MYSQL_ALLOW_EMPTY_PASSWORD=true -v /var/lib/mysql mysql`

Is going to give a name to the volume:

`docker container run -d --name mysql_new_with_volume -e MYSQL_ALLOW_EMPTY_PASS
WORD=true -v mysql-db:/var/lib/mysql mysql`

`docker volume ls`

```
DRIVER    VOLUME NAME
local     78aa3dcc06c9a61e26ed02357322748bab88a9487a3a14f9b11060609b566bc3
local     e090c91765f711d003c1837ae41680c1f289851138fcf5d21faf57bfe29a2363
local     ecde94ee71e9bc45dcf6dea15ed81dd61bb4a8416ca9032b7aeb6e6e7208a417
local     mysql-db
```

`docker volume inspect mysql-db`

```
[
    {
        "CreatedAt": "2023-03-07T07:35:59Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/mysql-db/_data",
        "Name": "mysql-db",
        "Options": null,
        "Scope": "local"
    }
]

```

`docker volume create` - required to do this before 'docker run' to use custom drivers and labels