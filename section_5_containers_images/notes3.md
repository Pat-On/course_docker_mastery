# Building Images: The Dockerfile Basic

`package manager` - PM' like apt and yum are one of the reasons to build containers from debian, ubuntu, fedora or CentOs

`Environment variables` - one reason they were chosen as preferred way to inject key/value is they work everywhere, on every os and config

# Building images: Running Docker Builds

`docker image build -t customnginx .`

- on the top of dockerfile keep things that not changing or changing rarely
- on the bottom of dockerfile keep the things that changing frequently

# Building Images: Extending Official Images

- you can chain images together and inherit even the CMD for example in the sample 2
  this is inherit from the nginx

Related to sample 2
`docker image build -t nginx-with-html .`

# Using Prune to Keep Your Docker System Clean (YouTube)

You can use "prune" commands to clean up images, volumes, build cache, and containers. Examples include:

- docker image prune to clean up just "dangling" images

- docker system prune will clean up everything

- The big one is usually docker image prune -a which will remove all images you're not using. Use docker system df to see space usage.

Remember each one of those commands has options you can learn with --help.

Here's a YouTube video I made about it: https://youtu.be/_4QzP7uwtvI
