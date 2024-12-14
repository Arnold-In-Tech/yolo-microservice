
# Description
An e-commerce website that doubles-up as a dashboard where you can load retail products on to the site.

# Overview
This project involved the containerization and deployment of a full-stack yolo application using Docker.

# Technology stack
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white)
![NPM](https://img.shields.io/badge/NPM-%23CB3837.svg?style=for-the-badge&logo=npm&logoColor=white)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)
![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)


------------------------------------------------ 

# Requirements
Install the docker engine here:
- [Docker](https://docs.docker.com/engine/install/) 
- [Node.js and Node.js package manager (npm)](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04)  
- [MongoDB](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/)

# How to test-run the application locally

## Clone the repo

```sh
git clone https://github.com/Arnold-In-Tech/yolo-microservice
```

------------------------------------------------ 

# Start backend

Check your operating system's built-in init system used to run and manage your mongod process:

```console
foo@bar:~$ ps --no-headers -o comm 1
systemd
```

Start MongoDB. 
```sh
sudo systemctl start mongod || sudo service mongod start 
```

Verify that MongoDB has started successfully.
```sh
sudo systemctl status mongod
```

Start server
```sh
cd ./yolo-microservice/backend
npm install
npm start
```

------------------------------------------------ 

# Start front-end

Start client
```sh
cd ./client
npm install
```

If using nodeV17 and above, update environment:
```sh
export NODE_OPTIONS=--openssl-legacy-provider
```

else
```sh
npm start
```

------------------------------------------------

# Orchestration using Docker compose

- Install docker compose in your local computer here: [Docker compose](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04)

- Generate a configuration file [docker-compose.yaml](./docker-compose.yaml) that builds both front-end and backend microservices (images, containers, volumes, networks, communication ports).

- Generate Dockerfiles for the back-end ( [Dockerfile](./backend/Dockerfile) ) and front-end ( [Dockerfile](./client/Dockerfile) ) containers to assemble dependencies, appropriate ports, and run the executables for starting the application.

- On your local computer terminal, to build images before starting containers run:
```sh
sudo docker-compose build  
```
 
- To start services
```sh
sudo docker-compose up  
```

------------------------------------------------

# Images repositories
Images built can be accessed on Dockerhub here:
[Frontend](https://hub.docker.com/r/ahnoamu/ahnoamu-yolo-client)
[Frontend](https://hub.docker.com/r/ahnoamu/ahnoamu-yolo-backend)


# Author 
Arnold. A.

# Date
2024

