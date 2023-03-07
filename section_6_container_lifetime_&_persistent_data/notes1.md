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

