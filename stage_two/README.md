
# README for AWS Deployment with Terraform and Ansible

This document provides a step-by-step guide for deploying infrastructure on AWS using Terraform to provision resources and Ansible playbooks to configure the servers.

## Technology stack
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![AWS](https://img.shields.io/badge/Amazon_AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Ansible](https://img.shields.io/badge/Ansible-000000?style=for-the-badge&logo=ansible&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)


## 1. Initialize Terraform Modules with AWS Credentials

Before starting, add your AWS access and secret keys to `main.tf`. Once the keys are added, initialize the Terraform modules with:

```bash
terraform init
```

## 2. Create Security Groups

Define the necessary security groups to control network access to your instances. Ensure your security groups are properly set in the Terraform configuration.

## 3. Create Virtual Machines (Instances) from an AMI Image

After defining the security groups and resource instances in `main.tf`, run the following commands:

```bash
terraform plan    # This checks for any issues in your Terraform configuration
terraform apply   # This deploys the virtual machines to AWS
```

## 4. Generate SSH Key Pair

On a new terminal, generate an SSH key pair:

```bash
ssh-keygen   # Run this once to avoid overwriting an existing key
```

## 5. Retrieve the Public Key

Navigate to the `.ssh` directory to find your generated key pair:

```bash
cd ~/.ssh
ls
cat id_rsa.pub  # Display the public key
```

## 6. Public and Private Key Pairs

Two key files are generated: a public key (`id_rsa.pub`) and a private key (`id_rsa`). The public key is what you'll use for authentication to the cloud servers, as Ansible connects via SSH.

## 7. Copy the Public Key to the Cloud Servers

To enable SSH access, copy the public key to your AWS instances. To do this, follow these steps:

1. Go to the AWS Management Console, select the instance, and click **Connect**.
2. Use the username `ubuntu` to connect to the instance (this will open the server terminal).

## 8. Add the Public Key to the Instance's `authorized_keys`

Once connected to the instance:

```bash
cd ~/.ssh
ls                # You should see the 'authorized_keys' file
sudo nano authorized_keys  # Open the file for editing
# Paste the public key here and save the file
```

## 9. Repeat for the Second Server

Perform the same process for the second server, adding the public key to its `authorized_keys` file as well.

## 10. Register Hosts in Ansible Inventory

Add the IP addresses of your servers to the Ansible hosts file. You can find the IP addresses in the instance summary in the AWS Management Console.

```bash
# Example format for adding hosts in your Ansible inventory:
[servers]
server1 ansible_host=XX.XX.XX.XX
server2 ansible_host=YY.YY.YY.YY
```

## 11. Configure Ansible Settings

In your `ansible.cfg`, ensure the following configurations:

- Inventory file points to the correct hosts.
- Remote user is set to `ubuntu`.
- Private key file is located at `~/.ssh/id_rsa`.

Example:

```ini
[defaults]
inventory = ./hosts
remote_user = ubuntu
private_key_file = ~/.ssh/id_rsa
```

## 12. Create and Configure the Ansible Playbook

Create a playbook (`playbook.yml`) that performs the following tasks:

1. Tests server connectivity.
2. Updates the server.
3. Installs required packages (e.g., Docker, pip).
4. Uses Ansible roles to pull Docker images from Docker Hub and create containers.
5. Starts applications within the respective containers.

## 13. Deploy the Playbook to the Servers

Run the following command to provision your infrastructure and deploy the necessary configuration:

```bash
ansible-playbook playbook.yml
```

## 14. Access the Deployed Application

After the playbook runs successfully, the website will be available at the IP address of the first server (e.g., `server-1` IP). The second server will have a similar setup:

- http://54.173.108.17/
OR
- http://54.168.100.112/


## 15. Destroy AWS Resources to Avoid Charges

Once you’re done and no longer need the resources, run the following command to destroy the infrastructure and avoid further charges:

```bash
terraform destroy
```

This will clean up all the resources deployed by Terraform.

---

## Author 
Arnold. A.

## License
This project is licensed under the MIT License - see the [LICENSE](../LICENSE.md) file for details.
