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

- Activate configuration by linking to the config file from Nginx’s sites-enabled directory:

$ sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/

- This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing:

$ sudo nginx -t
You shall see following message:

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
If any errors are reported, go back to your configuration file to review its contents before continuing.

We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

sudo unlink /etc/nginx/sites-enabled/default
When you are ready, reload Nginx to apply the changes:

$ sudo systemctl reload nginx

- Create an index.html file in that location so that we can test that your new server block works as expected:

sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html

- Testing PHP with Nginx:
$ nano /var/www/projectLEMP/info.php

Type the following lines into the new file. This is valid PHP code that will return information about your server:
<?php
phpinfo();
- Visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:
- ![project2f](https://user-images.githubusercontent.com/40290711/115116377-123d1780-9f91-11eb-806c-b1c72ebd3968.PNG)

- Retrieving data from MySQL database with PHP
+First, connect to the MySQL console using the root account:

$ sudo mysql

- To create a new database, run the following command from your MySQL console:
mysql> CREATE DATABASE `example_database`; 

- creates a new user and password:
mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';

- Give this user permission: 
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';

- Now exit the MySQL shell with:
mysql> exit

- You can test if the new user has the proper permissions:
$ mysql -u example_user -p

- Confirm that you have access to the example_database database:
mysql> SHOW DATABASES;

- Create a test table named todo_list:
CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );

- Insert row into table:
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");

-To confirm that the data was successfully saved to your table, run:
mysql>  SELECT * FROM example_database.todo_list;

-Exit the MySQL console:
mysql> exit

-Create a new PHP file in your custom web root directory:
$ nano /var/www/projectLEMP/todo_list.php

-Copy this content into your todo_list.php script:

<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}

- Save and close:
- Cntrl + x

-Visiting the domain name or public IP address configured for your website, followed by /todo_list.php:

http://<Public_domain_or_IP>/todo_list.php

![project2g](https://user-images.githubusercontent.com/40290711/115116989-5c73c800-9f94-11eb-9ac9-df382a38796b.PNG)




![project2h](https://user-images.githubusercontent.com/40290711/115117002-685f8a00-9f94-11eb-9350-8475672aaf99.PNG)



![project2i](https://user-images.githubusercontent.com/40290711/115117010-72818880-9f94-11eb-98e7-039fb681b97a.PNG)



![project2j](https://user-images.githubusercontent.com/40290711/115117013-7d3c1d80-9f94-11eb-9b17-1b6749c4f463.PNG)



![project2k](https://user-images.githubusercontent.com/40290711/115117025-83ca9500-9f94-11eb-8a5d-122011e34689.PNG)



