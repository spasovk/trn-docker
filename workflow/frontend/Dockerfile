FROM node:16-alpine as builder
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "run", "start"]

# define the ngin container node
FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html
