# Objective: Implement a Client Server Architecture using MYSQL Database Management System (DBMS).

## STEPS
===============

- Create and cinfigure two Linux-based virtual servers (EC2 instances in AWS):

![image](https://user-images.githubusercontent.com/40290711/127063939-8772db42-389e-40a2-bee0-9ac1e2724cad.png)

- Install MyQSL server on the mysql server AWS EC2 instance : sudo apt install mysql-server

![projectserver](https://user-images.githubusercontent.com/40290711/127065836-5565c572-d7a0-42ee-8142-4fb6cd9c8f4a.PNG)


- Do the same for the mysql client AWS EC2 instance: sudo apt install mysql-client

![projectserver](https://user-images.githubusercontent.com/40290711/127066029-c2eeb90f-2a85-4e4e-a123-671bd0b5d6e5.PNG)

- Enable the system using: sudo systemctl enable mysql
- 
![image](https://user-images.githubusercontent.com/40290711/127067157-23dcb548-1148-4036-88ec-36752f5d9b98.png)

-By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’: 

![image](https://user-images.githubusercontent.com/40290711/127067668-9f4fa2b8-62bb-4b1d-a643-dc9c4253d91a.png)

- Secure the sql server using: sudo mysql_secure_installation:

![projectserver4](https://user-images.githubusercontent.com/40290711/127068534-7b7728bc-58f2-4ac3-9134-756befeb958b.PNG)

- Start the MySql server using: sudo mysql

- Create a user on the MySQL server:

![projectserver5](https://user-images.githubusercontent.com/40290711/127069209-374df21d-2809-45f7-b589-c60184abb31e.PNG)


- Create a Database on the MySQL server: 
![projectserver6](https://user-images.githubusercontent.com/40290711/127069815-9c47b2d7-375f-4018-97a6-d323ea2bf3f5.PNG)



