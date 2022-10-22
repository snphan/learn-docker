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

 
