# What's In An Image (and what isn't)

Section about:

- All about images, the building block of containers
- What is in an image (and what is not)
- Using Docker Hub registry
- managing our local image cache
- building our own images

---

Interesting Links:
https://www.bretfisher.com/kubernetes-vs-docker/
https://opencontainers.org/about/overview/
https://blog.aquasec.com/a-brief-history-of-containers-from-1970s-chroot-to-docker-2016
https://opencontainers.org/
https://docs.google.com/spreadsheets/d/1ZT8m4gpvh6xhHYIi4Ui19uHcMpymwFXpTAvd3EcgSm4/edit#gid=0
https://docs.docker.com/desktop/
https://github.com/mikegcoleman/docker101/blob/master/Docker_eBook_Jan_2017.pdf
https://www.youtube.com/watch?v=sK5i-N34im8&list=PLBmVKD7o3L8v7Kl_XXh3KaJl9Qw2lyuFl
https://www.bretfisher.com/docker-for-mac-commands-for-getting-into-local-docker-vm/
https://www.bretfisher.com/getting-a-shell-in-the-docker-for-windows-vm/
https://www.digitalocean.com/community/tutorials/package-management-basics-apt-yum-dnf-pkg
https://docs.docker.com/config/formatting/

---

https://www.oracle.com/cloud/networking/dns/
https://howdns.works/
https://en.wikipedia.org/wiki/Round-robin_DNS
https://docs.docker.com/storage/storagedriver/
https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

---

### What is in an image and what is not

- App binaries and dependencies
- Metadata about the image data and how to run the image
- Official Definition: - An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime
- Not a complete OS. No kernel, kernel modules (e.g. drivers)
- Small as one file (your app binary) like a golang static binary
- Big as a Ubuntu distro with apt, and Apache, PHP and more installed

---

# The Mighty Hub: Using Docker Hub Registry Images

The lecture

- Basic of Docker Hub
- Find Official and other good public images
- Download images and basics of images tags

The Lecture: Review

- Docker Hub, 'The apt package system for containers'
- official images and how to use them
- how to discern good public images
- using different base images like debian and alpine

---

# Images and Their Layers: Discover The Image Cache

This Lecture:

- image layers
- union file system - layers around the changes
- `history` and `inspect` commands
- copy on write

`docker image ls`

`docker image history <name>` - Show layers of changes made in image

`docker history nginx:latest`

```
IMAGE          CREATED      CREATED BY                                      SIZE      COMMENT
dcfda10b61c5   2 days ago   /bin/sh -c #(nop)  CMD ["mongod"]               0B                  <-- this is image,
                                                                                        lower parts are just the layers>
<missing>      2 days ago   /bin/sh -c #(nop)  EXPOSE 27017                 0B
<missing>      2 days ago   /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B
<missing>      2 days ago   /bin/sh -c #(nop) COPY file:e3d6a34db83fe880…   14.2kB
<missing>      4 days ago   /bin/sh -c #(nop)  ENV HOME=/data/db            0B
<missing>      4 days ago   /bin/sh -c #(nop)  VOLUME [/data/db /data/co…   0B
<missing>      4 days ago   /bin/sh -c set -x  && export DEBIAN_FRONTEND…   551MB
<missing>      4 days ago   /bin/sh -c #(nop)  ENV MONGO_VERSION=6.0.4      0B
<missing>      4 days ago   /bin/sh -c echo "deb [ signed-by=/etc/apt/ke…   116B
<missing>      4 days ago   /bin/sh -c #(nop)  ENV MONGO_MAJOR=6.0          0B
<missing>      4 days ago   /bin/sh -c #(nop)  ENV MONGO_PACKAGE=mongodb…   0B
<missing>      4 days ago   /bin/sh -c #(nop)  ARG MONGO_REPO=repo.mongo…   0B
<missing>      4 days ago   /bin/sh -c #(nop)  ARG MONGO_PACKAGE=mongodb…   0B
<missing>      4 days ago   /bin/sh -c set -ex;  export GNUPGHOME="$(mkt…   1.16kB
<missing>      4 days ago   /bin/sh -c mkdir /docker-entrypoint-initdb.d    0B
<missing>      4 days ago   /bin/sh -c set -ex;   savedAptMark="$(apt-ma…   3.49MB
<missing>      4 days ago   /bin/sh -c #(nop)  ENV JSYAML_VERSION=3.13.1    0B
<missing>      4 days ago   /bin/sh -c #(nop)  ENV GOSU_VERSION=1.16        0B
<missing>      4 days ago   /bin/sh -c set -eux;  apt-get update;  apt-g…   11.7MB
<missing>      4 days ago   /bin/sh -c set -eux;  groupadd --gid 999 --s…   329kB
<missing>      5 days ago   /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>      5 days ago   /bin/sh -c #(nop) ADD file:fb4c8244f4468cdd3…   77.8MB
<missing>      5 days ago   /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B
<missing>      5 days ago   /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B
<missing>      5 days ago   /bin/sh -c #(nop)  ARG LAUNCHPAD_BUILD_ARCH     0B
<missing>      5 days ago   /bin/sh -c #(nop)  ARG RELEASE                  0B
```

### Layers

```
Layer 3 sha 35645       ENV      Image 1
Layer 2 sha 2435        APP
Layer 1 sha 12345       Ubuntu
```

```

       Image 2                 Image 3

Layer 4 sha 123 Port
Layer 3 sha fee Copy            Layer 3 sha fee Copy
Layer 2 sha efe2 APT            Layer 2 sha efe2 APT
                Layer 1 Jessie

```

We can chose the same image as a the same base images for a different containers so we are not storing the same image data many times.

```Custom Image

image 1            |            image 2
Layer 4 Copy A     |            Copy B
                Layer 3 Port
                Layer 2 Apache
                Layer 1 Custom
```

CUSTOM IMAGE

```

                                                Container 3 (read and write layer on top of Apache images)
                                        Container 2 (read and write layer on top of Apache images)
                                Container 1 (read and write layer on top of Apache images)

                        Apache <- image (read only)
```

Starting container and changing file from image - `copy own rights`
so the changes are inside the copy - in the container - and only there that files are different

---

`docker image inspect` - return JSON metadata about the image

Output - just metadata!

```
[
    {
        "Id": "sha256:904b8cb13b932e23230836850610fa45dce9eb0650d5618c2b1487c2a4f577b8",
        "RepoTags": [
            "nginx:latest"
        ],
        "RepoDigests": [
            "nginx@sha256:aa0afebbb3cfa473099a62c4b32e9b3fb73ed23f2a75a65ce1d4b4f55a5c2ef2"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2023-03-01T18:43:12.914398123Z",
        "Container": "71a4c9a59d252d7c54812429bfe5df477e54e91ebfff1939ae39ecdf055d445c",
        "ContainerConfig": {
            "Hostname": "71a4c9a59d25",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.23.3",
                "NJS_VERSION=0.7.9",
                "PKG_RELEASE=1~bullseye"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"nginx\" \"-g\" \"daemon off;\"]"
            ],
            "Image": "sha256:6716b8a33f73b21e193bb63424ea1105eaaa6a8237fefe75570bea18c87a1711",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "DockerVersion": "20.10.23",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.23.3",
                "NJS_VERSION=0.7.9",
                "PKG_RELEASE=1~bullseye"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "sha256:6716b8a33f73b21e193bb63424ea1105eaaa6a8237fefe75570bea18c87a1711",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 141838643,
        "VirtualSize": 141838643,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/558c958ecb1b1da2945fa6056e9b06e7cc6312a277636d7a54676f0910b1977f/diff:/var/lib/docker/overlay2/f0ac6d617bfbe68cbf354113985de9d9362cbb50d69496739ebab5d1000d8c1d/diff:/var/lib/docker/overlay2/31e2d8f79bab2a61b384f8214c789976620cd7dfcc35cc794c5ef1282d1a5578/diff:/var/lib/docker/overlay2/ce5dcd9e7595cfae3ef01f7e2836e4beb0c15e3a8846bde59b494e227c3d3ce7/diff:/var/lib/docker/overlay2/8a40c6c1135b82f6229df622b756aa25bc2a4f2905467675dab4eb59f1e3507f/diff",
                "MergedDir": "/var/lib/docker/overlay2/49d531ef6b5a1902c444f2221a14a3393347d46fa5b6617dfa820e608b9f2094/merged",
                "UpperDir": "/var/lib/docker/overlay2/49d531ef6b5a1902c444f2221a14a3393347d46fa5b6617dfa820e608b9f2094/diff",
                "WorkDir": "/var/lib/docker/overlay2/49d531ef6b5a1902c444f2221a14a3393347d46fa5b6617dfa820e608b9f2094/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:650abce4b096b06ac8bec2046d821d66d801af34f1f1d4c5e272ad030c7873db",
                "sha256:4dc5cd799a08ff49a603870c8378ea93083bfc2a4176f56e5531997e94c195d0",
                "sha256:e161c82b34d21179db1f546c1cd84153d28a17d865ccaf2dedeb06a903fec12c",
                "sha256:83ba6d8ffb8c2974174c02d3ba549e7e0656ebb1bc075a6b6ee89b6c609c6a71",
                "sha256:d8466e142d8710abf5b495ebb536478f7e19d9d03b151b5d5bd09df4cfb49248",
                "sha256:101af4ba983b04be266217ecee414e88b23e394f62e9801c7c1bdb37cb37bcaa"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

---

#### Image and Their Layers: Review

- images are made up of file system changes and metadata
- Each layer is uniquely identified and only stored once on a host
- This saves storage space on host and transfer time on push/pull
- A container is just a single read/write layer on top of image
- `docker image history` and `inspect` commands can teach us
  0
