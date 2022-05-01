# 1. node-image 

Has docker compose file for geting a node-app container and redis container

- Docker file:
Fetches node:alpine as base image. It it an alpine that has the npn command
Creates working dir
Copies the current working dir inside the container
Installs all modules that are listed on package.json file and their dependencies
index.js defines the program that counts the visits

```
docker compose up
```
Runs a redis database container and a node-app
Because we are using a docker-compose we can easily make them talk to one another.
node-app will listen on 8081 > it will be exposed on localhost:4001 by docker see docker-copose
node-app will connect to redis on 8789. see index.js