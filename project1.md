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
+ http://<Public-IP-Address>:80  (Public-IP-Address>:80)
![image](https://user-images.githubusercontent.com/40290711/115018503-d67e5100-9eaf-11eb-896c-d52f25d63dea.png)
- Installing MySQL
- using ‘apt’ to acquire and install this software:
+ sudo apt install mysql-server
- Run a security script:
+  sudo mysql_secure_installation
- Log in to the MySQL:
+ sudo mysql
- To exit the MySQL console:
+ mysql> exit
- Installing PHP
+ To install these 3 packages at once, run:
$ sudo apt install php libapache2-mod-php php-mysql
- Check the version of the installed php package:
+ php -v
-  Creating a Virtual Host for your Website using Apache
+ Create the directory for projectlamp using ‘mkdir’ command as follows:
sudo mkdir /var/www/projectlamp
- Assign ownership of the directory with the $USER environment variable:
sudo chown -R $USER:$USER /var/www/projectlamp
- Followed by:
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>  
- To save and close the file, simply follow the steps below:

Hit the esc button on the keyboard
Type :
Type wq. w for write and q for quit
Hit ENTER to save the file
You can use the ls command to show the new file in the sites-available directory

$ sudo ls /etc/apache2/sites-available
You will see something like this
000-default.conf  default-ssl.conf  projectlamp.conf

-You can now use a2ensite command to enable the new virtual host:
$ sudo a2ensite projectlamp  

To disable Apache’s default website use a2dissite command , type:

$ sudo a2dissite 000-default
To make sure your configuration file doesn’t contain syntax errors, run:

$ sudo apache2ctl configtest
Finally, reload Apache so these changes take effect:

$ sudo systemctl reload apache2

 Create an index.html file in that location so that we can test that the virtual host works as expected:

sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
Now go to your browser and try to open your website URL using IP address:

http://<Public-IP-Address>:80

- Enable PHP on the website
- In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:
+ sudo vim /etc/apache2/mods-enabled/dir.conf
-Followed by:
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

Then:
+ :wq (Write and quite)
- reload Apache so the changes take effect:

+ $ sudo systemctl reload apache2

- Create a new file named index.php inside your custom web root folder:

$ vim /var/www/projectlamp/index.php

- This will open a blank file. Add the following text, which is valid PHP code, inside the file:

<?php
phpinfo();

