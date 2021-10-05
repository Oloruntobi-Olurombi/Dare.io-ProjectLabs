# Ansible Configuration Management - Automate Project 7 to 10

In Projects 7 to 10 we had to perform a lot of manual operations to set up virtual servers, install and configure the required software, and also deploy our web application.

This Project will make us appreciate DevOps tools even more by automating most of the routine tasks with Ansible Configuration Management, at the same time you will become confident at writing code using declarative language such as YAML.

### Ansible Client as a Jump Server (Bastion Host)

A Jump Server (sometimes also referred as a Bastion Host) is an intermediary server through which access to an internal network can be provided. That means, even DevOps engineers cannot SSH into the Web servers directly, and can only access it through a Jump Server - this provides better security and also reduces the attack surface.

### Install and configure Ansible on EC2 Instance
- Update the Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. It's this server we will use to run our playbooks.

![image](https://user-images.githubusercontent.com/40290711/136100922-0d4bcbf5-ccc9-4bed-905c-c1512eb319f0.png)

- We would be creating a repository on our GitHub account and we would call it ansible-config-mgt.

![image](https://user-images.githubusercontent.com/40290711/136101107-a2367f81-7e06-441c-8aeb-a7adaddf32d6.png)

- Instal Ansible

sudo apt update

sudo apt install ansible
