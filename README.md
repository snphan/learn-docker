# Learning Docker Notes

# 01 - What is Docker?

## What is a container?
- A way to package an application with all the necessary dependencies and configuration
- Containers live on a container repository
- We can also use docker hub for a bunch of containers such as mongoDB, nginx, redis. 

### These are the layers of a container:
- Application image (postgres:10.10)
- Intermediate images
- Consists of a alpine:3.10 layer base image

## How do containers help application development.
- Before: we would need to install all of the services for each developer. 
    - There were many steps and a lot could go wrong.
    - For example downloading mySQL from the website and setting up the root users on Windows

- Now: with the containers, you don't need to install any of the binaries. 
    - Configurations, startscript and a service (such as postgres) can be run with one command

## Deployment

- Before
    - Development team sends artifacts and services with the conf files to operations team.
    - The operation team configures everything on the server to get the product working.
    - Problems such as dependency version conflics and miscommunication between the teams may cause problems

- After
    - Package configuration, artifacts and dependicies inside of a container. 
    - Run a docker command. No environmental configuration needed on server.

 
# 02 - Getting Started

You can go onto docker hub (https://hub.docker.com/search?q=postgres) and download 
the container with

    docker run postgres


Show all of the running containers with

    docker ps

Note: A container is a running environment for the image.

# 03 - Docker vs Virtual Machine

- Docker virtualizes the application layer
- Virtual Machines virtualizes the application and kernel.

Thus, docker images are much smaller than VMs and docker images can run faster.


# 04 - Basic Commands


Pull an image

    docker pull <image name>

Check all of the images

    docker images

Run a container. Use **`-d`** to run a container in the background (detached mode).

    docker run <image name>

Use docker stop to stop a container (use docker ps to get the container ID)

    docker stop <container id>

Use docker start to start a container (use docker ps to get the container ID)

    docker start <container id>

Find all containers that are running/not running:

    docker ps -a

Note: to talk with your container, you need to open a port on the host to connect to the container. 
There can only be one port for one container. Typically, the port on the container is preset
by the image (postgres is 5432 for example) 

Use the -p flag so that we specify the port:

    docker run -p <host port>:<container port>

# 05 - Debugging a Container

We can use **`docker logs`** or **`docker exec -it`** to debug our containers. **`-it`** means interactive terminal.

    docker logs <container name>

    docker exec -it <container name> bash

Use **`-f`** to stream the logs

    docker logs <container name> -f

# 06 - Demo Project

1. Dev
1. CI/CD
1. Deployment


The situation is you are developing a JS app with MongoDB database.

MongoDB + JS app on docker -> git -> CI (Jenkins builds) Artifacts push -> Docker Repository -> dev server pulls all images  

# 07 - Docker Networks

We can take a look at the networks that we have be doing

    docker network ls

any container within a network can refer to another container by container name (not localhost:3000 for example.)

Create a network with

    docker network create <arbitrary network name>

Run then run docker containers with the **`--net`** option.

    docker run -d \
    -p 27017:27017 \
    -n mongodb \
    --net <network name> \
    mongo 

# 08 - Docker compose

We create a **`docker-compose.yml`** file that has all of our configurations. We can turn the following
command where we run a mongodb and mongodb-express in a .yml file:

```
# The database
docker run -d\ 
--name mongodb\
-p 27017:27017\
-e MONGO-INITDB_ROOT_USERNAME=admin\
-e MONGO-INITDB_ROOT_PASSWORD=password\
--net mongo-network\
mongo

# Mongo-express
docker run -d\ 
--name mongo-express\
-p 8081:8081\
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin\
-e ME_CONFIG_MONGODB_SERVER=mongodb\
--net mongo-network\
mongo
```

To

```
version: '3'
services:
    mongodb: # container name
        image: mongo
        ports:
            - 27017:27017
        environment:
            - MONGO-INITDB_ROOT_USERNAME=admin
            - MONGO-INITDB_ROOT_PASSWORD=password
    mongo-express:
        image: mongo-express
        ports:
            - 8080:8080
        environments:
            - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
            - ME_CONFIG_MONGODB_SERVER=mongodb
    
```

Note that we don't need to specify a network! Docker compose handles this.
