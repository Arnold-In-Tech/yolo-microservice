
# Yolo microservice application: IP2

## Description
An e-commerce website that doubles-up as a dashboard where you can load retail products on to the site.

## Overview
This project involved the containerization and deployment of a full-stack yolo application using Docker.

## Technology stack
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white)
![NPM](https://img.shields.io/badge/NPM-%23CB3837.svg?style=for-the-badge&logo=npm&logoColor=white)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)
![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)


------------------------------------------------ 

## Requirements
Install the docker engine here:
- [Docker](https://docs.docker.com/engine/install/) 
- [Node.js and Node.js package manager (npm)](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04)  
- [MongoDB](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/)


## Test-run the application locally

### Clone the repo

```sh
git clone https://github.com/Arnold-In-Tech/yolo-microservice
```

------------------------------------------------ 

### Start backend

   - Check your operating system's built-in init system used to run and manage your mongod process:

```console
foo@bar:~$ ps --no-headers -o comm 1
systemd
```

   - Start MongoDB. 
```sh
sudo systemctl start mongod || sudo service mongod start 
```

   - Verify that MongoDB has started successfully.
```sh
sudo systemctl status mongod
```

   - Start server
```sh
cd ./yolo-microservice/backend
npm install
npm start
```

------------------------------------------------ 

### Start front-end

   - Start client
```sh
cd ./client
npm install
```

   - If using nodeV17 and above, update environment first:
```sh
export NODE_OPTIONS=--openssl-legacy-provider
```

   - else
```sh
npm start
```

------------------------------------------------

## Build docker images and push to github
------------------------------------------------ 

   - Build back-end

```sh
cd ./backend
sudo docker build -t ahnoamu/ahnoamu-yolo-backend:v1.0.1 .
```

   - Build front-end

```sh
cd ./client
sudo docker build -t ahnoamu/ahnoamu-yolo-client:v1.0.1 .
```

### Test-run built images: run images in a container

   - Run back-end container

```sh
cd ./backend
sudo docker run --name backend-container -d -it  --rm -p 5000:5000 ahnoamu/ahnoamu-yolo-backend:v1.0.1
```

   - Run front-end container

```sh
cd ./client
sudo docker run --name frontend-container -d -it  --rm -p 3000:3000 ahnoamu/ahnoamu-yolo-client:v1.0.1
```

Check port 3000 to ascertain that the website is active (localhost:3000)

### Push docker images to dockerhub

```sh
cd ./backend
sudo docker push ahnoamu/ahnoamu-yolo-client:v1.0.1
cd ../client
sudo docker push ahnoamu/ahnoamu-yolo-backend:v1.0.1
cd ..
```

------------------------------------------------ 

## Orchestration using Docker compose

------------------------------------------------ 

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

## Images repositories
Images built can be accessed on Dockerhub here:
- [Frontend](https://hub.docker.com/r/ahnoamu/ahnoamu-yolo-client)
- [Backend](https://hub.docker.com/r/ahnoamu/ahnoamu-yolo-backend)



#
# Configuration management IP3

# Local Ubuntu VM Deployment with Vagrant and Ansible

This guide explains how to deploy the Yolo e-commerce app to a local Ubuntu 20.04 virtual machine using Vagrant for infrastructure provisioning and Ansible playbooks to configure and deploy the application.

## Technology stack
![Vagrant](https://img.shields.io/badge/Vagrant-1868F2?style=for-the-badge&logo=Vagrant&logoColor=white)
![Ansible](https://img.shields.io/badge/Ansible-000000?style=for-the-badge&logo=ansible&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)


## A: Create and Configure Ubuntu 20.04 Virtual Machine

### 1. List Installed Vagrant Machines
To view the list of available Vagrant boxes:

```sh
vagrant box list
```

### 2. Add Jeff Geerlings’ Ubuntu 20.04 64-bit Box
Download and add the `geerlingguy/ubuntu2004` box:

```sh
vagrant box add geerlingguy/ubuntu2004
```

Choose the **virtualbox** option when prompted.

### 3. Initialize a New Virtual Machine Configuration
Create a new Vagrant configuration for the Ubuntu 20.04 box:

```sh
vagrant init geerlingguy/ubuntu2004
```

This will generate a `Vagrantfile` that configures the virtual machine using the downloaded box.

### 4. Configure Hosts and Ansible
Edit the `hosts` and `ansible.cfg` files with the following:

- Add the server name, `ansible_host` (IP address), `ansible_port`, and `ansible_user` (usually `vagrant`).
- Set `host_key_checking = False` in `ansible.cfg` to avoid issues with authentication keys or certificates.

### 5. Boot the Virtual Machine
Start your Ubuntu server with the following command:

```sh
vagrant up
```

This will bring up the virtual machine and make it ready for provisioning.

## B: Set Up the Ansible Playbook

### 1. Set Up the Playbook to Dockerize the Yolo E-Commerce App
Create a playbook that will install the necessary packages (Docker, pip3) on the virtual machine and deploy the Yolo e-commerce app.

### 2. Directory Structure for Ansible Roles
The playbook will be organized into roles for deploying the backend, frontend, and MongoDB components of the application. The directory structure looks like this:

```
yolo-microservice/
├── playbook.yml                # Main playbook
├── roles/                      # Contains all roles
│   ├── backend-deployment/    
│   │   ├── tasks/              # Tasks for backend deployment
│   │   └── vars/               # Variables for backend deployment
│   ├── frontend-deployment/   
│   │   ├── tasks/              # Tasks for frontend deployment
│   │   └── vars/               # Variables for frontend deployment
│   └── setup-mongodb/         
│       ├── tasks/              # Tasks for MongoDB setup
│       └── vars/               # Variables for MongoDB setup
└── inventory/                  # Inventory of hosts
```

### 3. Role Actions

#### Backend and Frontend Deployment
- **Pulls the backend and frontend repositories** from Docker Hub:
  - [Frontend Image](https://hub.docker.com/r/ahnoamu/ahnoamu-yolo-client)
  - [Backend Image](https://hub.docker.com/r/ahnoamu/ahnoamu-yolo-backend)
  
- **Ensures the network 'app-net'** exists for inter-container communication.
- **Creates Docker containers** for both the backend (Node.js) and frontend.
  - The backend image comes pre-installed with the necessary dependencies like Node.js and npm.
  
- **Defines network name and access ports** for each container.
- **Starts the backend and frontend applications**.

#### MongoDB Setup
- **Pulls the MongoDB image** from Docker Hub and stores it in `/home/vagrant` directory.
- **Creates a MongoDB container**.
- **Defines access ports, volumes, and network settings**.
- **Starts the MongoDB container** for database services.

## C: Run the Playbook to Automatically Install and Start Containers

Once the virtual machine is up and the playbook is configured, run the following command to provision the VM and start the application containers:

```sh
vagrant provision
```

This will install Docker, pull the required images, create the necessary containers, and start the application.

### 4. Access the Application
You can access the Yolo e-commerce app by visiting the following URLs in your browser:

- http://172.17.0.1:3000/
OR
- http://localhost:3000/

These URLs will point to the application running on your local Ubuntu VM.


## Author 
Arnold. A.

## License
This project is licensed under the MIT License - see the [LICENSE](./LICENSE.md) file for details.

