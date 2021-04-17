#WEB STACK IMPLEMENTATION (LEMP STACK) IN AWS LEMP STACK is a technology stack that stands for Linux, Nginx, MySQL and PHP or Python, or Perl. Steps in deploying a LEMP STACK using AWS EC2 instance and Git Bash:

- I created a new EC2 instance for LEMP STACK (As the Instance that I used for project1 failed the status Nginx test)
- Connect into EC2 instance using the git bash with the following command:
+ ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
- Installing the Nginx Web Server:
+ $ sudo apt update
+ $ sudo apt install nginx
- Verify that nginx was successfully installed and is running as a service in Ubuntu, run:
+ $ sudo systemctl status nginx

![project2a](https://user-images.githubusercontent.com/40290711/115115269-6fce6580-9f8b-11eb-9efc-0ff0e2408553.PNG)

- Create a new inbound rule (HTTP : 80) in the security group section. 
- Check out server locally in Ubuntu shell, run:
+ $ curl http://localhost:80 

![project2b](https://user-images.githubusercontent.com/40290711/115115486-9a6cee00-9f8c-11eb-9be0-408ab046af41.PNG)

- Nginx server can respond to requests from the Internet:
(http://<Public-IP-Address>:80)

- Installing MySQL:
+ $ sudo apt install mysql-server 
![project2c](https://user-images.githubusercontent.com/40290711/115115736-f08e6100-9f8d-11eb-80e4-fae8a53e0930.PNG)

- This following script will remove some insecure default settings and lock down access to database system:
+ sudo mysql_secure_installation

- Installing PHP:
+ sudo apt install php-fpm php-mysql

-  Configuring Nginx to Use PHP Processor:
-Create the root web directory for your_domain as follows:
$ sudo mkdir /var/www/projectLEMP  

- Assign ownership of the directory with the $USER environment variable, which will reference current system user:
$ sudo chown -R $USER:$USER /var/www/projectLEMP

- Open a new configuration file in Nginx’s sites-available directory using preferred command-line editor. Here, we’ll use nano:

$ sudo nano /etc/nginx/sites-available/projectLEMP
This will create a new blank file. Paste in the following bare-bones configuration:

#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}

