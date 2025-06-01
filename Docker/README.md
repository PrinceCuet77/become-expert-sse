- [Docker](#docker)
  - [Container](#container)
  - [Image](#image)
    - [Commands](#commands)
  - [Docker Image Layers](#docker-image-layers)
  - [Port Binding](#port-binding)
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

- 37:12

![Port Binding](photo/port-binding.png)

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
