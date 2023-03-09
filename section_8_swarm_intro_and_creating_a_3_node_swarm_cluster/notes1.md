# Containers Everywhere = New Problems

- How do we automate container lifecycle?
- How can we easily scale out in up down?
- How can we ensure our containers are re-created if they fail?
- How can we replace containers without downtime (blue/green deploy)?
- How can we control/track where containers get started?
- How can we create cross-node virtual networks?
- How can we ensure only trusted servers run our containers?
- How can we store secrets, keys, passwords and get them to the right container (and only that container)?

# Swarm Mode: Built-in Orchestration

- Swarm Mode is a clustering solution built inside Docker
- Not related to Swarm 'classic' for pre-1.12 versions
- Added in 1.12 (summer 2016) via SwarmKit toolkit
- Enhanced in 1.13 (January 2017) via Stacks and Secrets
    - docker swarm
    - docker node
    - docker service
    - docker stack
    - docker secret 