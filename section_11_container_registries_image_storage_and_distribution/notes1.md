# 90 Docker Hub: Digging dipper

- an image registry needs to be part of your container plan
- More Docker Hub details including auto-build
- How Docker Store (store.docker.com) is different then hub
- How Docker Cloud (cloud.docker.com) is different then Hub
- use new Swarms feature in Cloud to connect Mac/Win to swarm
- install and use Docker Registry as private image store
- 3rd party registry options

---

- the most popular public image registry
- it is really Docker Registry plus lightweight image building (extra staff)
- lets explore more of the features of docker hub
- link Github/Bitbucket to hub and auto-build image on commit
- chain image building together

---

# 91 Understanding Docker Registry

- A private image registry for your network
- Part of the docker/distribution GitHub repo
- The de facto in private containers registries
- not as full featured as Hub or other, no web ui, basic auth only
- At it is core: a web API and storage system, written in Go
- Storage supports local, s3/azure/alibaba/google Cloud and OpenStack Swift

---

## Running Docker Registry Cont:

- Look in section resources for links to:
- secure your registry with TLS
- storage cleanup via Garbage Collection
- enable hub caching via `--registry-mirror`
  - proxy mode! Interesting!

Links:
https://docs.docker.com/registry/configuration/
https://docs.docker.com/registry/garbage-collection/
https://docs.docker.com/registry/recipes/mirror/
