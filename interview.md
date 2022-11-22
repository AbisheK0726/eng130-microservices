# Test

```dockerfile
#FROM nginx
#LABEL MAINTAINER=aneese-docker-app
#COPY app /home/
#COPY environment /home/
#EXPOSE 80
#EXPOSE 3000
#RUN apt-get update
#RUN apt-get install -y
#RUN apt-get install software-properties-common -y
#RUN apt-get install npm -y
#CMD ["nginx", "-g", "daemon off;"]
#WORKDIR /home/app
#RUN npm install
#CMD ["npm", "start"]

From node as app
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install -g npm@7.20.6
Copy . .
EXPOSE 3000
CMD ["node", "app.js"]

FROM node:alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install -g npm@7.20.6
Copy --from=app /usr/src/app /usr/src/app
EXPOSE 3000
CMD ["node", "app.js"]
