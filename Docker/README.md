- [Docker](#docker)
  - [Container](#container)
  - [Image](#image)
    - [Commands](#commands)
  - [Docker Image Layers](#docker-image-layers)
  - [Port Binding](#port-binding)
  - [Troubleshoot Commands](#troubleshoot-commands)
  - [Docker vs VM](#docker-vs-vm)
  - [Docker Network](#docker-network)
    - [Setup `mongo` \& `mongo-express`](#setup-mongo--mongo-express)
  - [Docker Compose](#docker-compose)
  - [Dockerizing Our App](#dockerizing-our-app)
  - [All Docker Commands](#all-docker-commands)
    - [Images](#images)
    - [Container](#container-1)
    - [Troubleshoot](#troubleshoot)
    - [Docker Hub](#docker-hub)
    - [Volumes](#volumes)
    - [Network](#network)

# Docker

- Remove the installation dificulties of machine to machine dependencies

![Why Use Docker](photo/why-use-docker.png)

## Container

- Combined package of the application with it's dependencies, makes a single unit called container

![Docker Container](photo/docker-container.png)

- Share the container means share the application with it's dependencies
- Replicating the entire local development in a standarize way accross the large team

![Share Container](photo/share-container.png)

- Container properties
  - Portable (Share container as image)
  - Light weight (lower size as local installation)
- Usecase: Same local environment but use 2 different version of NodeJS as Docker Container

![Multiple Containers](photo/multiple-containers.png)

## Image

- Executable file that has the instruction to build single/multiple containers
- Image is a blueprint that helps us to build a multiple container like class-object relationship
- Share image not container
- Image will execute in the local machine & create container(s)
- Container will use the system resources
- In a word:
  - Container - Running instance
  - Image - Static snapshot/screenshot that local machine should look like

![Docker Image](photo/docker-image.png)

### Commands

- Pull an image from DockerHub of latest version if I don't mention the version

```cmd
docker pull <image_name>
```

![pull](photo/terminal-photo/p1.png)

- List all the local images

```cmd
docker images
```

![images](photo/terminal-photo/p2.png)

- `TAG` is basically a version of that image
- Create & run a new container

```cmd
docker run <image_name>
```

![run](photo/terminal-photo/p3.png)

- Create & run a new container in interactive mood using `-it` flag
- Helps us to input & output anything

```cmd
docker run -it <image_name>
```

![run (-it)](photo/terminal-photo/p4.png)

- List all Local containers (running & stopped)

```cmd
docker ps -a
```

![ps -a](photo/terminal-photo/p5.png)

- List all running containers

```cmd
docker ps
```

![ps](photo/terminal-photo/p6.png)

- Start an existing container using `container_name` or `container_id`

```cmd
docker start <container_name>
docker start <container_id> // Alternative
```

![start](photo/terminal-photo/p7.png)

- Stop an existing container

```cmd
docker stop <container_name>
docker stop <container_id> // Alternative
```

![stop](photo/terminal-photo/p8.png)

- Delete an image

```cmd
docker rmi <image_name>
docker rmi <image_id> // Alternative
```

- Delete a container

```cmd
docker rm <container_name>
docker rm <container_id> // Alternative
```

- Before removing an image, must remove the container

![rm & rmi](photo/terminal-photo/p9.png)

- Pull a specific version of image from DockerHub

```cmd
docker pull <image_name>:<version>
```

![pull with version](photo/terminal-photo/p10.png)

- Run the image in the background mode (detach mode)
- By default, all the container will run in the attach mode

```cmd
docker run -d <image_name>
```

- For `mysql`, I can use environment variables to run the command

![run -d -e](photo/terminal-photo/p11.png)

- I can give the custom name of any container

```cmd
docker run --name <container_name> -d <image_name>
```

![run --name](photo/terminal-photo/p12.png)

## Docker Image Layers

- When I create a new container, a new writable layer (called container layer) is added on top of underlying layers
- Other layers are read only

![Docker Image Layers](photo/docker-image-layers.png)

- If a layer is already present in the local machine, then it use that instead of downloading again

![pull mysql](photo/terminal-photo/p13.png)

## Port Binding

- Bind the docker port with the local machine's port

![Port Binding](photo/port-binding.png)

- Without binding the port showing `3306/tcp` for both

![Port not-binding](photo/terminal-photo/p14.png)

- Port Binding in container

```cmd
docker run -p<host_port>:<container_port> <image_name>
```

![Port Binding](photo/terminal-photo/p15.png)

## Troubleshoot Commands

- Fetch logs of a container which can identify the root cause of any problem if exist

```cmd
docker logs <container_name>
docker logs <container_id> // Alternative
```

![logs](photo/terminal-photo/p16.png)

- Using `exec`, execute any additional command
- Open shell inside running container

```cmd
docker exec -it <container_name> /bin/bash
docker exec -it <container_name> sh
```

![exec](photo/terminal-photo/p17.png)

## Docker vs VM

- Docker
  - Docker uses host os kernel
  - And virtualize the application layer
  - As only virtualize application layer, so it's light weight
  - Initially, docker is only running in the linux based OS
  - But with the help of docker desktop, docker is able to run in mac & windows
  - As docker desktop has linux based VM inside on it
- VM
  - Virtualize host os kernel & application layer both
  - So, compatible with all types of OS (mac, linux, windows)

![Docker vs VM](photo/docker-vs-vm.png)

## Docker Network

- I have a backend and frontend
- For database, I will use mongodb which is in the docker container
- Use `mongo` & `mongo-express` both docker images
- `mongo` is the database docker image but `mongo-express` is the docker image for web view for that `mongo` database

![Server Container Connection](photo/server-container-connection.png)

- I need to wrap that both docker containers within a docker network
- As, interact both docker container with each other without any ports & limitatins
- So, I need docker network
- Create a docker network & setup 2 containers on it to interact

![Docker Network](photo/docker-network.png)

- List all networks

```cmd
docker network ls
```

- Create a network

```cmd
docker network create <network_name>
```

![Create Docker Network](photo/create-docker-network.png)

- Remove a network

```cmd
docker network rm <network_name>
```

![Network rm](photo/network-remove.png)

- Remove all unused networks

```cmd
docker network prune
```

### Setup `mongo` & `mongo-express`

- `-d` for detach mode (not taking the whole terminal for `mongo`)
- `-p27017:27017` for port binding
- `--name mongo` rename as mongo
- `--network mongo-network` setup inside the `mongo-network`
- `-e ROOT_USERNAME=admin` setup the user name
- `-e ROOT_PASSWORD=qwerty` setup the password

```cmd
docker run -d \
-p27017:27017 \
--name mongo \
--network mongo-network \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=qwerty \
mongo
```

![mongo](photo/terminal-photo/p18.png)

```cmd
docker run -d \
-p8081:8081 \
--name mongo-express \
--network mongo-network \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty \
-e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017" \
mongo-express
```

![mogno-express](photo/terminal-photo/p19.png)

- For mongodb url breakdown

```cmd
-e ME_CONFIG_MONGODB_URL="mongodb://<username>:<password>@<container-name>:<port>
```

- Open the terminal and visit `localhost:8081` & give the username as `admin` and password as `pass`

![Application](photo/testapp-mongo.png)

## Docker Compose

- Instead of running long docker container command in the terminal, execute all the commands by including in a single file
- So, docker compose is a tool for defining and running multi-container applications
- Advantages:
  - More flexible to edit the commands
  - Structured and standarize format
- `services` defines the containers
- Docker command vs Docker compose

![Docker Compose](photo/docker-compose.png)

- So, equivalent docker compose file is

```yaml
version: '3.8'

services:
  mongo:
    image: mongo:latest
    ports:
      - 27017:27017
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: qwerty

  mongo-express:
    image: mongo-express:latest
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: qwerty
      ME_CONFIG_MONGODB_URL: mongodb://admin:qwerty@mongo:27017/
```

- Don't mention any network, as by default docker compose will create a network and containers are start in that network
- Docker container create & start command

```cmd
docker compose -f filename.yaml up -d
```

![Compose Up](photo/compose.png)

- Docker container remove permanently command

```cmd
docker compose -f filename.yaml down
```

![Compose Down](photo/compose-down.png)

## Dockerizing Our App

- Convert our app in the docker image using `Dockerfile`

![Dockerize](photo/dockerize.png)

## All Docker Commands

### Images

- List all Local images

```cmd
docker images
```

- Delete an image

```cmd
docker rmi <image_name>
```

- Remove unused images

```cmd
docker image prune
```

- Build an image from a Dockerfile

```cmd
docker build -t <image_name>:<version> . // version is optional
docker build -t <image_name>:<version> . -no-cache // build without cache
```

### Container

- List all Local containers (running & stopped)

```cmd
docker ps -a
```

- List all running containers

```cmd
docker ps
```

- Create & run a new container
- If image not available locally, it’ll be downloaded from DockerHub

```cmd
docker run <image_name>
```

- Run container in background

```cmd
docker run -d <image_name>
```

- Run container with custom name

```cmd
docker run - -name <container_name> <image_name>
```

- Port Binding in container

```cmd
docker run -p<host_port>:<container_port> <image_name>
```

- Set environment variables in a container

```cmd
docker run -e <var_name>=<var_value> <container_name>
docker run -e <var_name>=<var_value> <container_id> // Alternative
```

- Start an existing container

```cmd
docker start <container_name>
docker start <container_id> // Alternative
```

- Stop an existing container

```cmd
docker stop <container_name>
docker stop <container_id> // Alternative
```

- Inspect a running container

```cmd
docker inspect <container_name>
docker inspect <container_id> // Alternative
```

- Delete a container

```cmd
docker rm <container_name>
docker rm <container_id>
```

### Troubleshoot

- Fetch logs of a container

```cmd
docker logs <container_name>
docker logs <container_id> // Alternative
```

- Open shell inside running container

```cmd
docker exec -it <container_name> /bin/bash
docker exec -it <container_name> sh
```

### Docker Hub

- Pull an image from DockerHub

```cmd
docker pull <image_name>
```

- Publish an image to DockerHub

```cmd
docker push <username>/<image_name>
```

- Login into DockerHub

```cmd
docker login -u <image_name>
```

- Or

```cmd
docker login
```

- Also, docker logout to remove credentials
- Search for an image on DockerHub

```cmd
docker search <image_name>
```

### Volumes

- List all Volumes

```cmd
docker volume ls
```

- Create new Named volume

```cmd
docker volume create <volume_name>
```

- Delete a Named volume

```cmd
docker volume rm <volume_name>
```

- Mount Named volume with running container

```cmd
docker run - -volume <volume_name>:<mount_path>
```

- Or using - -mount

```cmd
docker run - -mount type=volume,src=<volume_name>,dest=<mount_path>
```

- Mount Anonymous volume with running container

```cmd
docker run - -volume <mount_path>
```

- To create a Bind Mount

```cmd
docker run - -volume <host_path>:<container_path>
```

- Or using - -mount

```cmd
docker run - -mount type=bind,src=<host_path>,dest=<container_path>
```

- Remove unused local volumes

```cmd
docker volume prune //for anonymous volumes
```

### Network

- List all networks

```cmd
docker network ls
```

- Create a network

```cmd
docker network create <network_name>
```

- Remove a network

```cmd
docker network rm <network_name>
```

- Remove all unused networks

```cmd
docker network prune
```
