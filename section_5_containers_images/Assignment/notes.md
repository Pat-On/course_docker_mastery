# Assignment: Build Your Own Image

- Dockerfiles are part process workflow and part art <3
- Take existing Node.js app and Dockerize it
- Make Dockerfile. Build it. Test it. Push it. (rm it). Run it.
- Expect this to be iterative. Rarely do I get it right the first time.
- Details in dockerfile-assignment-1/Dockerfile
- Use the Alpine version of the official node 6.x image
- Expected result is web site at http://localhost
- Tag and push to your docker hub account (free)
- Remove your image from local cache, run again from Hub

# Solution

Commands:
`docker image build -t http_test_image .`
`docker container run -p 80:3000 --name httpTest2 -d http_test_image`
`docker tag http_test_image patvon/http_test_image`
`docker push patvon/http_test_image`

`docker image rm patvon/http_test_image`
`docker container run --rm -p 80:3000 patvon/http_test_image`

Answer from video:

```
FROM node:6-alpine

EXPOSE 3000

RUN apk add --update tini

RUN mkdir -p /usr/src/app

WORKDIR /usr/src/app

COPY package.json package.json

// second job depends from first
RUN npm install && npm cache clean

// second job do not depend from the first
// RUN npm install; npm cache clean

COPY . .

CMD ["/sbin/tini", "--", "node", "./bin/www"]

```
