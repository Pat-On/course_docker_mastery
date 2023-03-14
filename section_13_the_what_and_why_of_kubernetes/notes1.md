# 100 What is Kubernetes

- Kubernetes = popular container orchestrator
- Container Orchestration = make many servers act like one
- Released byu Google in 2015, maintained by large community
- Runs on top of Docker (usually) as a set of APIs in containers
- provides API/CLI to manage container across servers
- many clouds provide it for you
- many vendors make a distribution of it

Links:
https://en.wikipedia.org/wiki/Kubernetes
https://kubernetes.io/partners/#conformance

# 101 Why Kubernetes

- Orchestration: next logical step in journey to faster dev-ops
- first, understand why you may need orchestration
- not every solution needs orchestration
- servers + change rate = benefit of orchestration
- Then, decide which orchestrator
- if kubernetes, decide which distribution
  - cloud or self-managed (docker enterprise, rancher, openshift, canonical vmware pks)
  - do not usually need pure upstream - you need to implement many extra features by yourself

link: https://kubernetes.io/partners/#conformance

# 102 Kubernetes vs Swarm
- Review "Swarm Mode: Build-in Orchestration"
- Kubernetes and Swarm are both containers orchestrators
    - both of them run docker containers 
- both are solid platforms with vendor backing
- swarm: easier to deploy/manage
- kubernetes: more features and flexibility
- What is right for you? understand both and know your requirements


## Advantages of swarm
- comes with docker, single vendor container platform 
- easiest orchestrator to deploy/manage yourself
- Follows 80/20 rules, 20% of features for 80% of use cases
    - not scientific just hint ^^
    - local, cloud, datacenter
    - ARM, Windows, 32-bit
- Secure by default
- easiest to troubleshoot - less complicated

## Advantages of Kubernetes
- Clouds will depoloy/manage Kubernetes for you
- Infrastructure vendors are making their own distribution
- widest adoption and community
- flexible: covers widest set of use cases
- Kubernetes first vendor support
- no one ever got fired for buying IBM
    - picking solutions is not 100% rational
    - trendy, will benefit your career
    - CIO/CTO Checkbox

