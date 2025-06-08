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
  - [Publishing Images](#publishing-images)
  - [Docker Volumes](#docker-volumes)
  - [Container Orchestration](#container-orchestration)
    - [Docker Swarm](#docker-swarm)
    - [Kubernetes](#kubernetes)
  - [All Docker Commands](#all-docker-commands)
    - [Images](#images)
    - [Container](#container-1)
    - [Troubleshoot](#troubleshoot)
    - [Docker Hub](#docker-hub)
    - [Volumes](#volumes)
    - [Network](#network)
  - [Notes](#notes)

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

- Stop and Remove All Containers

```cmd
docker rm -f $(docker ps -aq)
```

- Breakdown:
  - `docker ps -aq`: Lists all container IDs (running or stopped).
  - `docker rm -f`: Force removes all listed containers.
- Remove All Images

```cmd
docker rmi -f $(docker images -q)
```

- Breakdown:

  - `docker images -q`: Lists all image IDs.
  - `docker rmi -f`: Force removes all listed images.

- Delete a container

```cmd
docker rm <container_name>
docker rm <container_id> // Alternative
```

- Delete all the stopped containers

```cmd
docker container prune
```

- Additional details for a specific container in a JSON format

```cmd
docker inspect <container-name>
```

- See the history of an image

```cmd
docker history <image-name>
```

- Image build command

```cmd
docker build .
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
- By default, the driver is `bridge` type

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

- `-d` - for detach mode (not taking the whole terminal for `mongo`)
- `-p27017:27017` - for port binding
- `--name mongo` - rename as mongo
- `--network mongo-network` - setup inside the `mongo-network`
- `-e ROOT_USERNAME=admin` - setup the user name
- `-e ROOT_PASSWORD=qwerty` - setup the password

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

- Use `bridge` network driver to comunicate with other `bridge` network driver containers in the same host machine
- `host` network driver use same network as our host machine
- So, there is not IP Address
- So, container will create with no IP Address and use the same network which is already used in host machine
- For `null` network driver is isolated from host machine itself & other container as well

![network ls](photo/terminal-photo/p29.png)

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

- Important `Dockerfile` instructions
- `FROM` - define the base image
- `WORKDIR` - working directory or the path in the image where files will be copied & commands will be executed
- `COPY` - copy files from host to the image folder
- `RUN` - run the instructions, can use multiple times
- `CMD` - run the application, can use only time time in the `Dockerfile`
- `EXPOSE` - expose the port
- `ENV` - setup the environment variables

![Dockerfile](photo/dockerfile.png)

- The `Dockerfile` is:

```Dockerfile
FROM node

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=qwerty

RUN mkdir -p testapp

COPY . /testapp

CMD [ "node", "/testapp/server.js" ]
```

- Build the docker image from `Dockerfile`

```cmd
docker build -t <new_image_name>:<version> .
```

![build](photo/terminal-photo/p20.png)

- Run the already created docker image

```cmd
docker run <new_image_name>:<version>
docker run -it <new_image_name>:<version> // interactive mode
```

![run](photo/terminal-photo/p21.png)

## Publishing Images

- Create an account to docker hub
- Create a repository
- Build my custom image in the terminal
- Login that account through the terminal

```cmd
docker logout
docker login -u <username>
```

![Docker Hub Login](photo/docker-hub-login.png)

- Push my custom made image to the docker hub

```cmd
docker push <new_image_name>
```

![push](photo/terminal-photo/p22.png)

- In the docker hub

![Docker Hub Image Repo](photo/docker-hub-my-image.png)

- Now, customize the `yaml` file to pull my newly pushed image from the docker hub

```yaml
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

  testapp:
    image: princecuet77/testapp:latest
    ports:
      - 5000:5000
```

- Run the docker compose file in the terminal

![Docker Compose](photo/terminal-photo/p23.png)

## Docker Volumes

- Volumes are persistent data stores for containers
- The volume is reserved in the host system and managed by docker

![Docker Volume](photo/volume.png)

- Mount Named volume with running container

```cmd
docker run -v <volume_name>:<mount_path> <image_name>
```

![volume](photo/terminal-photo/p24.png)

- So, even I stop or delete the container, this 2 files will be present
- Volume mount using `.yaml` file

```yaml
services:
  mongo:
    image: mongo:latest
    ports:
      - 27017:27017
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: qwerty
    volumes:
      - /home/prince/Desktop/data:/data/db # Here

  mongo-express:
    image: mongo-express:latest
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: qwerty
      ME_CONFIG_MONGODB_URL: mongodb://admin:qwerty@mongo:27017/

  testapp:
    image: princecuet77/testapp:latest
    ports:
      - 5000:5000
```

- List all Volumes

```cmd
docker volume ls
```

![volume ls](photo/terminal-photo/p25.png)

- Create new Named volume
- Named volume is isolated or not attached to any running container

```cmd
docker volume create <volume_name>
```

![volume create](photo/terminal-photo/p26.png)

- By default for windows, docker will create the volume in:

```txt
C:\ProgramData\docker\volumes
```

- By default for mac/linux, docker will create the volume in:

```txt
/var/lib/docker/volumes
```

- Delete a Named volume

```cmd
docker volume rm <volume_name>
```

![volume rm](photo/terminal-photo/p27.png)

- Mount Named volume with running container
- Most popular to use
- If my given `volume_name` is exist then use otherwise will create a new one in default mac/linux/windows path
- Created & managed by docker itself
- Similar three commands

```cmd
docker run -v <volume_name>:<mount_path/container_dir>
docker run --volume <volume_name>:<mount_path/container_dir>
docker run --mount type=volume,src=<volume_name>,dest=<mount_path>
```

- Mount with anonymous volumes
- For any temporary storage use
- Created & managed by docker itself as well
- Similar three commands

```cmd
docker run -v <mount_path/container_dir>
docker run --volume <mount_path/container_dir>
docker run --volume <host_path>:<container_path>
```

- To create a Bind Mount
- Similar three commands

```cmd
docker run -v <host_dir>:<mount_path/container_dir>
docker run --volume <host_dir>:<mount_path/container_dir>
docker run --mount type=bind,src=<host_path>,dest=<container_path>
```

- To remove unused docker volumes
- Mainly target anonymous docker volumes

```cmd
docker volume prune
```

![volume prune](photo/terminal-photo/p28.png)

## Container Orchestration

- Docker orchestration is essential when you're managing multiple containers across one or more hosts in a reliable, scalable, and automated way.
- Here’s why orchestration matters:
  - Automated Container Management
  - Load Balancing and Service Discovery
  - Scaling Applications
  - Multi-Container Apps
  - Multi-Host Deployment
  - Security and Configuration
- A container orchestration allows to deploy hundreds or thousands of instances of my application with a single command
- There are multiple container orchestration solutios
  - Docker Swarm
  - Kubernetes from Google
  - Mesos from Apache

### Docker Swarm

- Docker swarm combine multiple docker machines together into a single cluster 

### Kubernetes

- A

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

- Build Args and Custom Dockerfile

```cmd
docker build -t <image-name>:<version> -f Dockerfile.prod --build-arg ENV=production .
```

- Removes dangling images only — these are images not tagged and not referenced by any container.
- Images created during intermediate builds that aren’t used anymore.

```cmd
docker image prune
```

- Removes all unused images, not just dangling ones.
- This includes any image not used by at least one container, even if it is tagged.

```cmd
docker image prune -a
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
docker run --name <container_name> <image_name>
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
docker run --volume <volume_name>:<mount_path>
```

- Or using `--mount`

```cmd
docker run --mount type=volume,src=<volume_name>,dest=<mount_path>
```

- Mount Anonymous volume with running container

```cmd
docker run --volume <mount_path>
```

- To create a Bind Mount

```cmd
docker run --volume <host_path>:<container_path>
```

- Or using `--mount`

```cmd
docker run --mount type=bind,src=<host_path>,dest=<container_path>
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

- By default, created `bridge` network, if I specify

```cmd
docker run --network=host <container-name>
docker run --network=none <container-name>
```

- Remove a network

```cmd
docker network rm <network_name>
```

- Remove all unused networks

```cmd
docker network prune
```

## Notes

- [kobekloud - Docker(Beginner)](https://notes.kodekloud.com/docs/Docker-Training-Course-for-the-Absolute-Beginner/Introduction/Introduction)
- [Example Voting App for Docker Compose](https://github.com/dockersamples/example-voting-app)
