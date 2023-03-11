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

# 81 Using Secrets in Swarm Service

Link: https://docs.docker.com/engine/swarm/secrets/

## using file

`docker secret create psql_user psql_user.txt`

- drawback storring data on your server

## from the command line:

`echo "myDBpassWORD" | sudo docker secret create psql_pass -`

`-` - dash command is telling cmd to read from the std input

- history of the bash command

`sudo docker secret ls`

````
ID NAME        DRIVER    CREATED              UPDATED
vf0ko6bgvs3tm6naoalcf69pz   psql_pass             About a minute ago   About a minute ago
y87eydpqz42023003lcx9v5fo   psql_user             2 minutes ago        2 minutes ago```
````

`sudo docker secret inspect psql_user`

```
[
    {
        "ID": "y87eydpqz42023003lcx9v5fo",
        "Version": {
            "Index": 313
        },
        "CreatedAt": "2023-03-11T18:00:25.164126568Z",
        "UpdatedAt": "2023-03-11T18:00:25.164126568Z",
        "Spec": {
            "Name": "psql_user",
            "Labels": {}
        }
    }
]
```

only containers and services are going to have access to it

# Manual creation of the service

`docker service create --name psql --secret psql_user --secret psql_pass -e POSTGRES_PASSWORD_FILE=/run/secrets/psql_pass -e POSTGRES_USER_FILE=/run/secrets/psql_user postgres`

these POSTGRES_PASSWORD_FILE and POSTGRES_USER_FILE is build in into official images

`sudo docker exec -it  psql.1.ka0zq6sboy6zmswy1c6rlr8ca bash`

# removing rsecrets

`docker service update --secret-rm `

if i would remove the secret then containers with this secret would be removed and reinitialized.
The reason of it is because we have immutable objects/containers
.