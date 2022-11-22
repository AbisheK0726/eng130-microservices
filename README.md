# Microservices with Docker and Kubernetes

![Microservices](images/microservices-diagram.png)

## Microservices

### What are microservices?

Microservices are a software development technique—a variant of the service-oriented architecture (SOA) architectural style that structures an application as a collection of loosely coupled services, in a service-oriented architecture, services are fine-grained and the protocols are lightweight.

### Why microservices?

Microservices are a way to break up an application into small, independent services. Each service is a small application that runs in its own process and communicates with other services using a lightweight mechanism, often an HTTP resource API. These services are built around business capabilities and independently deployable by fully automated deployment machinery. There is a bare minimum of centralized management of these services, which may be written in different programming languages and use different data storage technologies.

### What are the benefits of microservices?

| Benefits | Description |
| --- | --- |
| **Scalability** | Microservices are small and independent, so they can be scaled up or down independently. |
| **Resilience** | If one microservice fails, the others remain unaffected. |
| **Technology diversity** | Microservices can be written in different programming languages and use different data storage technologies. |
| **Loose coupling** | Microservices are loosely coupled, so they can be developed, tested, and deployed independently. |

## Docker and Kubernetes

![Docker-Kubernetes](images/docker_architecture.svg)

### What is Docker?

Docker is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels. All containers are run by a single operating-system kernel and are thus more lightweight than virtual machines.

### Why Docker?

Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux/Windows machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

## Kubernetes

### What is Kubernetes?

Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications. It groups containers that make up an application into logical units for easy management and discovery. Kubernetes builds upon 15 years of experience of running production workloads at Google, combined with best-of-breed ideas and practices from the community.

### Why Kubernetes?

Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem. Kubernetes services, support, and tools are widely available.

## Virtualization vs Containerization

![Virtualization vs Containerization](images/virtualization-vs-containerization.png)


## Docker Setup

### Docker Installation - Windows

1. Download the Docker Desktop Installer from [here](https://hub.docker.com/editions/community/docker-ce-desktop-windows).
2. Run the installer and follow the instructions.
3. Install WSL 2 backend by following the instructions [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
4. Once the installation is complete, open the Docker Desktop application.
5. Restart your computer.

### Alias for Docker

1. Open the Git Bash as an administrator.
2. Run the following command to create an alias for docker

```bash
alias docker="winpty docker"
```

### Docker Commands

```bash
# List all running containers
docker ps

# List all containers
docker ps -a

# List all images
docker images

# Build an image
docker build -t <image-name>

# Remove an image
docker rmi <image-id>

# Remove all images
docker rmi $(docker images -q)

# Run a container
docker run -p <host-port>:<container-port> <image-name>

# Run a container in detached mode
docker run -d -p <host-port>:<container-port> <image-name>

# Enter a container
docker exec -it <container-id> bash

# Stop a container
docker stop <container-id>

# Remove a container
docker rm <container-id>

# Remove all containers
docker rm $(docker ps -a -q)

# See the logs of a container
docker logs <container-id>
```

### Docker push image to Docker Hub

Commit the changes to docker hub

```bash
docker commit <container-id> <docker-hub-username>/<image-name>
```

Push the image to docker hub

```bash
docker push <docker-hub-username>/<image-name>
```

### Dockerfile

```dockerfile
# Specify a base image and tag
FROM nginx
LABEL MAINTAINER=abishek726

# Copy files from the host to the container
# COPY <source> <destination>
COPY index.html /usr/share/nginx/html

# Port to expose
EXPOSE 80

# Run a command
CMD ["nginx", "-g", "daemon off;"]
```

Run the following commands to build and run the image

```bash
# Build an image
docker build -t <image-name> .

# Run a container
docker run -p <host-port>:<container-port> <image-name>
```

### Setup NodeApp on Docker via AWS

1. Create an EC2 instance with Node App AMI.
2. SSH into the instance.
3. Run the following commands to install Docker

```bash
# Update the apt package index and install docker
sudo apt-get update
curl -fsSL https://get.docker.com -o get-docker.sh
sudo snap install docker
```

Check the docker version

```bash
docker --version
```

4. Create a Dockerfile in working directory of the app folder and add the following content

```dockerfile
FROM nginx
LABEL MAINTAINER=aneese-docker-app
COPY app /home/
COPY environment /home/
EXPOSE 80
EXPOSE 3000
RUN apt-get update
RUN apt-get install -y
RUN apt-get install software-properties-common -y
RUN apt-get install npm -y
CMD ["nginx", "-g", "daemon off;"]
WORKDIR /home/app
RUN npm install
CMD ["npm", "start"]
```

5. Build the image

```bash
docker build -t <image-name> .
```

6. Run the container

```bash
docker run -p <host-port>:<container-port> <image-name>
```

7. Visit the public IP of the instance to see the app running.

8. Push the image to Docker Hub

### Docker MongoDB

1. Create a Dockerfile in working directory of the app folder and add the following content

```dockerfile
FROM mongo:latest

WORKDIR /usr/src/db/

COPY ./mongod.conf /etc/

EXPOSE 27017

CMD ["mongod"]
```

### Docker Compose

Dockers compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

1. Create a docker-compose.yml file in the working directory of the app folder and add the following content

```yml
services:
  db:
    image: mongo
    # Mapping of container port to host
    ports:
      - "27017:27017"


  app:
  # Path to Dockerfile
    build: ./app
    restart: always
    ports:
      - "3000:3000"
    environment:
      - DB_HOST=mongodb://localhost:27017/posts
    depends_on:
      - db
```
