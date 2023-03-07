FROM node:6-alpine

# use this empty Dockerfile to build your assignment
# NOTE: The instructions are now in the README.md file in this directory.

RUN apk add --no-cache tini

WORKDIR /usr/src/app

COPY . .

RUN npm install && npm cache clean --force

EXPOSE 3000

CMD ["/sbin/tini", "--", "node", "./bin/www"]