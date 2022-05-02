# 1. node-image 

Has docker compose file for geting a node-app container and redis container

- Dockerfile:
Fetches node:alpine as base image. It is an alpine that has the npn command
Creates working dir.
Copies the current working dir inside the container.
- package.json - used by docker to install all modules that are listed and their dependencies.
- index.js defines the program that counts the visits

```
docker compose up
```
Runs a redis database container and a node-app
Because we are using a docker-compose we can easily make them talk to one another.
node-app will listen on 8081 > it will be exposed on localhost:4001 by docker see docker-copose
node-app will connect to redis on 8789. see index.js

2. ![workflow / frontend](workflow/frontend)
- Install Create React App globally and generate the application
```
npx create-react-app frontend
```
Useful commands
```
npm run test    - run test on a project
npm run build   - build a production version of out application
npm run start   - starts a development server
```
- Create Dockerfile.dev
```
FROM node:16-alpine
WORKDIR '/app'
COPY package.json .
RUN npm install

COPY . .

CMD ["npm", "run", "start"]
```
It will work as is but its not handy so we will do a docker compose file.
- Create docker compose - docker-compose.yml
```
version: '3'
services: 
  web:
    build: 
      context: .
      dockerfile: Dockerfile.dev
    ports: 
      - "3000:3000"
    volumes:
      - /app/node_modules
      - .:/app
  # Not needed but basically test container and the host will share the same volume
  tests:
    build: 
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - /app/node_modules
      - .:/app
    command: ["npm", "run", "test"]
```

# The workflow
1. Build phase
  - run node:alpine16
  - ocopy the package.json
  - install dependancies
  - Run 'npm build'
2. Run phase
  - use nginx image
  - copy the result of 'npm build'
  - start nginx

  Dockerfile:
  ```
FROM node:16-alpine as builder
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "run", "start"]

# define the ngin container node
FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html
  ```
