# 80 Secrets Storage for Swarm:

## Protecting Your Environment Variables

Secret Storage

- easiest secure solution for storing secrets in swam
- the easiest because it is build into the swarm
- What is a Secret

  - Username and password
  - TLS certificates and keys
  - SSH keys
  - Any data you would prefer not be "on front page of news"

- Supports generic strings or binary content up to 500Kb in size
- Does not require apps to be re-written

Secrets Storage Cont.

- As of Docker 1.13.0 Swarm Raft DB is encrypted on disk
- only stored on disk on Manager nodes
- Default is Managers and Workers 'control plane' is TLS + Mutual Auth
- Secrets are first stored in Swarm, then assigned to a Service(s)
  - basically we are letting to know docker who is allowed to use these secrets
- Only containers in assigned Service(s) can see them
- They look like files in containers but are actually in-memory fs
- `/run/secrets/<secret_name>` or `/run/secrets/<secret_alias>`
- local docker compose can use file-based secrets, but not secure! !IMPORTANT TO REMEMBER
  - it is actually faking the secure storage
