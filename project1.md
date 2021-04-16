#WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
LAMP STACK is a technology stack that stands for Linux, Apache, MySQL and PHP or Python, or Perl.

Steps in deploying a LAMP STACK using AWS EC2 instance:
- Login to my AWS account.
- Access the EC2 dashboard via the console management services.
- Lauch a EC2 virtual server with Ubuntu Server OS.
- I installed puTTY on my local computer.
- used puTTYGen to convert private key file from .PEM to .PPK.
- Connect into EC2 instance using puTTY.
- Installing Apache and Updating the Firewall:
+ Install Apache using Ubuntu’s package manager ‘apt’:
+  sudo apt update (To update)
+  sudo apt install apache2 (To install apache2)
- To verify that apache2 is running as a Service in our OS, use following command:
+ sudo systemctl status apache2
- create an inbound rule to connect EC2 and Apache via port 80 
- Access server locally via ubuntu shell using:
+ curl http://localhost:80 
- Access via the internent using:
+ http://<Public-IP-Address>:80 
