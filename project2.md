#WEB STACK IMPLEMENTATION (LEMP STACK) IN AWS LEMP STACK is a technology stack that stands for Linux, Nginx, MySQL and PHP or Python, or Perl. Steps in deploying a LEMP STACK using AWS EC2 instance and Git Bash:

- I created a new EC2 instance for LEMP STACK (As the Instance that I used for project1 failed the status Nginx test)
- Connect into EC2 instance using the git bash with the following command:
+ ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
- Installing the Nginx Web Server:
+ $ sudo apt update
+ $ sudo apt install nginx
- Verify that nginx was successfully installed and is running as a service in Ubuntu, run:
+ $ sudo systemctl status nginx
