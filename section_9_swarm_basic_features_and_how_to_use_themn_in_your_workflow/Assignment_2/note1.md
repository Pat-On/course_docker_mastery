# 83 Assignment: Create A stack with Secrets and Deploy

- Let's use our Drupal compose file from last assignment
  - (compose-assignment-2)
- rename image back to official `drupal:8.2`
- remove `build:`
- add secret via `external:`
- use environment variable `POSTGRES_PASSWORD_FILE`
- Add secret via cli `echo "<pw>" | docker secret create psql-pw -`
- copy compose into a new yml file on your Swarm node1

`echo "myDBpassWORD" | sudo docker secret create psql-pw -`

` sudo docker stack deploy -c docker-compose.yml drupal`