# Classrooms Dockerfile
FROM node:14

WORKDIR /1232_backend

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3232
CMD ["npm", "start"]
