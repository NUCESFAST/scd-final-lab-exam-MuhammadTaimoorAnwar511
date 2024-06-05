# Client Dockerfile
FROM node:14

WORKDIR /1232_frontend

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 1232
CMD ["npm", "start"]
