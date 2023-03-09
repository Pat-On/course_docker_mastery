# Assignment Build A compose file

- Build a basic compose file for a Drupal content management system website. Docker Hub is your friend
- Use the `drupal` image along with the postgres image
- Use `ports` to expose Drupal on 8080 so you can localhost:8080
- Be sure to set `POSTGRES_PASSWORD` for postgres
- Walk through Drupal setup via Browser
- Tip: Drupal assumes DB is `localhost`, but it is service name
- Extra Credit: Use volumes to store Drupal unique data

https://docs.docker.com/compose/compose-file/
https://docs.docker.com/compose/compose-file/#links
