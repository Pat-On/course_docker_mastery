# Assignment: named Volumes

- database upgrade with container
- Create a postgres container with named volume `psql-data` using version 9.6.1
- Use Docker Hub to learn `VOLUME` path and versions needed to run it
- Check logs, stop container
- Create a new postrgres container with same named volume using 9.6.2
- Check logs to validate - it is going to have less logs, because it is going to re-use db
- (this only works with patch versions, most SQL DB's require manual commands to upgrade DB's to major/minor version, i.e. it's DB limitation not a container one)

# Exercise answer:

`docker volume create psql`

`docker run -d --name psql1 -e POSTGRES_PASSWORD=mypassword -v mydb:/var/lib/postgresql/data postgres:15.1`
`docker logs psql1`
`docker stop psql1`

`docker run -d --name psql2 -e POSTGRES_PASSWORD=mypassword -v mydb:/var/lib/postgresql/data postgres:15.2`
`docker logs psql2`

```
PostgreSQL Database directory appears to contain a database; Skipping initialization

2023-03-08 06:32:25.213 UTC [1] LOG:  starting PostgreSQL 15.1 (Debian 15.1-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
2023-03-08 06:32:25.213 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2023-03-08 06:32:25.213 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2023-03-08 06:32:25.220 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2023-03-08 06:32:25.231 UTC [29] LOG:  database system was shut down at 2023-03-08 06:26:11 UTC
2023-03-08 06:32:25.242 UTC [1] LOG:  database system is ready to accept connections
```

`docker stop psql2`

`docker run -d --name psql5 -e POSTGRES_PASSWORD=mypassword -v psql postgres:15.2`

# Syntax

### In terms of syntax:

- If you type `<name>:/path/to/folder` then its a named volume
- If you type `/path/that/starts/with/slash:/path/to/folder` then its a bind mounts

### In terms of usage, it's more about who is going to use the volume.

`named volumes` are volumes that will be used solely by containers. It is stored within docker (example: /var/lib/docker/volumes/[volume_name]/\_data). It's basically a way to save your container data between kills

`bind mounts` are volumes for files/folders that you want to keep updated even for your host. That's why you specify where exactly you want it. It's a way of saying: my container can access those files over there, but they are also important for the host computer because it uses them too.

Interesting article:
https://cravencode.com/post/docker/create-named-docker-bind-mount/

