https://github.com/BretFisher/nodejs-rocks-in-docker
https://docs.docker.com/engine/reference/builder/#add

# 202 Real World PHP Dockerfile Review

- https://github.com/sebalaini/laravel_docker-compose
- https://github.com/cgb37/symfony4-docker-demo/blob/master/Dockerfile

### Sum

- reliable versions
- not separated run because of the caching
  - for example all apt-get run together
- multistage vs single stage
- on the top things that never or very rarely change
- dependencies that taking a lot of time to install to the top

- add - download something from the internet or unzip
- copy - work in one repo

- CMD and ENTRYPOINT - important because basically it describe what the image does

https://github.com/docker-library/ghost/blob/master/4/debian/Dockerfile

# 203 Real World PHP, Apache, and Alpine Docker Review

- hardcode versions!

# 204 Real World PHP and FPM Dockerfile Review
- https://github.com/nordnotor/docker-nginx-php/blob/master/7.1/alpine3.9/fpm/Dockerfile 
- read: https://docs.docker.com/build/building/multi-stage/#use-multi-stage-builds
- interesting separation of env

# 205 Real World Elasticsearch Compose Stack
- https://github.com/autopilotpattern
The autopilot pattern automates in code the repetitive and boring operational tasks of an application, including startup, shutdown, scaling, and recovery from anticipated failure conditions for reliability, ease of use and improved productivity.

