
# Yolo microservice application

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

## Orchestration using Docker compose

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
## A: Create and configure Ubuntu 20.04 virtual machine

- List installed machines
```sh
vagrant box list        
```


- Add  Jeff Geerlings ubuntu 20.04 64-bit ‘box’ using the vagrant box add command:

```sh
vagrant box add geerlingguy/ubuntu2004       # Select the “virtualbox” option.
```

- Then create a default virtual server configuration using the box you just downloaded.

```sh
vagrant init geerlingguy/ubuntu2004
```

- vagrant init command creates a Vagrant configuration file (Vagrantfile) for a Jeff Geerlings ubuntu 20.04 64-bit virtual machine image.

- Add the server name, ansible_host IP address, ansible_port, ansible_user to the hosts/ansible.cfg file

- Ensure no authentication keys or certificates are needed for ease of marking (host_key_checking = False)

- Boot up your Ubuntu server
```sh
vagrant up
```


## B: Set up the playbook 

- Set up a playbook to dockerize and run yolo e-commerce app using ansible

- Initially install required (docker, pip3) packages to your virtual machine

- The playbook contains 3 roles 

- Directory structure for roles:
```
yolo-microservice/
├── playbook.yml 
├── roles/                     # Contains all roles
│   ├── backend-deployment/    
│   │   ├── tasks/             # Tasks for the 'backend-deployment'
│   │   └── vars/              # Variables for the 'backend-deployment' role
│   ├── frontend-deployment/   
│   │   ├── tasks/             # Tasks for the 'frontend-deployment' role
│   │   └── vars/              # Variables for the 'frontend-deployment' role
│   └── setup-mongodb/         
│       ├── tasks/              # Tasks for the 'setup-mongodb' role
│       └── vars/               # Variables for the 'setup-mongodb' role
└── inventory/                  # Inventory of hosts
```

- Each role defines the tasks that will set up the docker containers generated previously. 

- The actions to be executed are defined in "main.yml"

### Actions for backed-deployment and frontend-deployment roles:
- Pulls the backend repository from Dockerhub
- Ensure the network 'app-net' exists
- Creates Node.js Backend Container from the pulled image
- The image is already installed with necessary dependencies (e.g., nodejs, npm).
- Defines access network name and ports 
- Starts the backend/frontend application.

### Actions for setup-mongodb role:
- Pulls the mongodb repository from Dockerhub into /home/vagrant
- Creates mongo image 
- Defines access ports, volumes, and network 
- Starts the database

## C: Run your playbook to automatically install and start containers:

```sh
vagrant provision
```
### The application can be accessed via http://172.17.0.1:3000/ or http://localhost:3000/


## Author 
Arnold. A.

## License
This project is licensed under the MIT License - see the [LICENSE](./LICENSE.md) file for details.

