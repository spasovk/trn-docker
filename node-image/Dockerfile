FROM node:alpine
# It will create the directorry if it doesn\t exists
WORKDIR '/app'
#Copy the working dir to the containerr working dir
COPY . .
RUN npm install
CMD ["npm", "run", "start"]