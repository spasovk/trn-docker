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