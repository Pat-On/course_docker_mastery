# 82 Using Secrets with Swarm Stacks

link: https://docs.docker.com/compose/compose-file/#secrets-configuration-reference

`docker service create --name search --replicas 3 -p 9200:9200 elasticsearch:2`

`docker stack deploy -c docker-compose.yml mydb`
output:

```
Creating network mydb_default
Creating secret mydb_psql_password
Creating secret mydb_psql_user
Creating service mydb_psql
```

`sudo docker secret ls`

```
ID                          NAME                 DRIVER    CREATED          UPDATED
onbyc3y1ne2z6wn9zmru7au5a   mydb_psql_password             36 seconds ago   36 seconds ago
xisvauwz6cb18b1m6b62hou7e   mydb_psql_user                 36 seconds ago   36 seconds ago
```

# entire stack would be gone after one command

`sudo docker stack rm mydb`

```
Removing service mydb_psql
Removing secret mydb_psql_password
Removing secret mydb_psql_user
Removing network mydb_default
```
