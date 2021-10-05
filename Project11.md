# Ansible Configuration Management 

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

![image](https://user-images.githubusercontent.com/40290711/136101359-4b6b9267-4a9c-413c-a50e-4ebb32b9ec60.png)

![image](https://user-images.githubusercontent.com/40290711/136101442-3b4b6001-9b59-4dc3-b814-fcae90ae8850.png)

- Check your Ansible version by running ansible --version

Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository:

![image](https://user-images.githubusercontent.com/40290711/136101645-d4397ce7-ed0b-4e92-9cb1-9fa000be1f7d.png)

- Configure Webhook in GitHub and set webhook to trigger ansible build:

![image](https://user-images.githubusercontent.com/40290711/136101754-3089d1eb-c15b-40fc-bfde-113a7c9f6a73.png)

- Go back to Jenkins and configure triggering a build job from GitHub webhook, and Configure “Post-build Actions” to archive all the files:

![image](https://user-images.githubusercontent.com/40290711/136102381-f611c399-380e-4f01-868b-78bf6155b839.png)

We are going to test our setup by making some change in the README.MD file in our main branch, and we would also make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder

/var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/

![image](https://user-images.githubusercontent.com/40290711/136102938-c45bb948-d4c2-4b7b-bc0a-5d8664a3a5fd.png)

From the above images, we can see that we have had 6 automatic builds in Jenkins, and they have also reflected in the folder as shown in the command below /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/

### Prepare the development environment using Visual Studio Code

I was stuck on connecting my VSCode IDE to my Jenkins-Ansible EC2 instance as I specified a private key (ppk) instead of a .pem key.

Follow this link to setup your VSCode to connect to your instance.

### Begin Ansible Development

In the ansible-config-mgt GitHub repository I created previously, we would be creating a new branch that will be used for development of a new feature.

We would checkout a newly created feature branch to our local machine and start building our code and directory structure

We first cloned the repository, then created a new feature using git checkout -b feature name

![image](https://user-images.githubusercontent.com/40290711/136103169-83a30268-2db2-4330-a16a-5ad778af1b89.png)

I created a directory and named it playbooks - it will be used to store all our playbook files.

I created a directory and named it inventory - it will be used to keep our hosts organised.

Within the playbooks folder, I created our first playbook, and named it common.yml

Within the inventory folder, I created an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

![image](https://user-images.githubusercontent.com/40290711/136103329-39c23daf-6918-435f-a2fe-2ed247b7347f.png)

### Set up an Ansible Inventory

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from the Jenkins-Ansible host - for this we need to copy our private (.pem) key and provide a path to it so Ansible can use it to connect. Lets not forget to change permissions to our private key chmod 400 key.pem, otherwise EC2 will not accept the key. Also notice, that our Load Balancer user is ubuntu and user for RHEL-based servers is ec2-user.

I made use of mobaxterm to copy the .pem key from my PC to the linux instance.

We would update our inventory/dev.yml file with this snippet of code:

[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>
<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[db]
<Database-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu' ansible_ssh_private_key_file=<path-to-.pem-private-key>
Below is what the dev.yml file looks like after inserting information into it

![image](https://user-images.githubusercontent.com/40290711/136103483-4b24d07f-2b93-492c-9382-3346681ca1c6.png)

### Create a Common Playbook
  
This is where we start giving Ansible the instructions on what is needed to be performed on all servers listed in inventory/dev.

In common.yml playbook we will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

We would update our playbooks/common.yml file with following code:
  
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
  - name: ensure wireshark is at the latest version
    yum:
      name: wireshark
      state: latest

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
  - name: ensure wireshark is at the latest version
    apt:
      name: wireshark
      state: latest

This playbook is divided into two parts, each of them is intended to perform the same task: install wireshark utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

### Update GIT with the latest code
  
Now all of our directories and files live on our machines and we need to push changes that we made locally to GitHub.

In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with help of GIT. In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes - it is also called “Four eyes principle”.

Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.

Commit code into GitHub:

use git commands to add, commit and push our branch to GitHub.
  
 git status

git add <selected files>

git commit -m "commit message"

git push --set-upstream origin feature name

![image](https://user-images.githubusercontent.com/40290711/136104105-5447026f-6a00-4c56-8880-669626fed10e.png)

In the above image, you can see that I made a mistake of not adding the commit command when i wanted to commit, hence my changes did not reflect on github.

Create a Pull request (PR)

  
