# Image Tagging and Pushing to Docker Hub

This Lecture:

- All about image tags
- How to upload to Docker Hub
- Image ID vs Tag

`docker image tag` - assign one or more tags to an image

[images technically have no names]

```
REPOSITORY               TAG             IMAGE ID       CREATED         SIZE
mongo                    latest          dcfda10b61c5   2 days ago      645MB
mariadb                  latest          6e11fcfc66ad   4 days ago      401MB
nginx                    latest          904b8cb13b93   4 days ago      142MB
```

`<user>/<repo>:<tag>` - Default tag is latest if not specified (unofficial)
`<repo>:<tag>` - official

Official Repositories - They live at the `root namespace` of the registry, so they do not need account name in front of repo name

```
REPOSITORY               TAG             IMAGE ID       CREATED         SIZE
mongo                    latest          dcfda10b61c5   2 days ago      645MB
mysql/mysql-server       latest          1d9c2219ff69   6 weeks ago     496MB
bretfisher/nodemongoapp   latest          58da083a347a   13 months ago   1GB
```

`docker pull nginx:mainline` - refer by name?

```
REPOSITORY                TAG             IMAGE ID       CREATED         SIZE
mongo                     latest          dcfda10b61c5   2 days ago      645MB
mariadb                   latest          6e11fcfc66ad   4 days ago      401MB
nginx                     latest          904b8cb13b93   4 days ago      142MB
nginx                     mainline        904b8cb13b93   4 days ago      142MB
```

`docker image tag nginx bretfisher/nginx`

`docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]`

```
REPOSITORY                TAG             IMAGE ID       CREATED         SIZE
mongo                     latest          dcfda10b61c5   2 days ago      645MB
mariadb                   latest          6e11fcfc66ad   4 days ago      401MB
bretfisher/nginx          latest          904b8cb13b93   4 days ago      142MB
nginx                     latest          904b8cb13b93   4 days ago      142MB
nginx                     mainline        904b8cb13b93   4 days ago      142MB
```

`Latest tag` = it is just the default tag, but image owners should assign it to the newest stable version

`docker image push` - uploads changed layers to a image registry (default is a Hub)

`docker login <server>` - Defaults to logging in Hub, but you can override by adding server url

`docker logout` - always logout from shared machines or servers when done, to protect your account

or

`docker image tag bretfisher/nginx bretfisher/nginx:testing`

```
REPOSITORY                TAG             IMAGE ID       CREATED         SIZE
mongo                     latest          dcfda10b61c5   2 days ago      645MB
mariadb                   latest          6e11fcfc66ad   4 days ago      401MB
bretfisher/nginx          latest          904b8cb13b93   4 days ago      142MB
bretfisher/nginx          testing         904b8cb13b93   4 days ago      142MB
nginx                     latest          904b8cb13b93   4 days ago      142MB
nginx                     mainline        904b8cb13b93   4 days ago      142MB
```

This Lecture: Review

- Properly tagging images
- Tagging images for upload to Docker Hub
- How tagging is r4elated to image ID
- The Latest Tag - it is just default tag and it is nothing special about it
- Logging into Docker Hub from docker cli
