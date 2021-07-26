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

- Grant permission on the database to the user using:
![projectserver7](https://user-images.githubusercontent.com/40290711/127070166-f19518d5-fc85-4da0-b055-1061b3748546.PNG)

- Then flush privileges using:

![projectserver8](https://user-images.githubusercontent.com/40290711/127070342-3a038c8b-d8bc-4ae9-8535-f917eaf6fddf.PNG)

- exit the MySQL server using: 

![projectserver9](https://user-images.githubusercontent.com/40290711/127070511-41873a7b-d2b1-481e-8de3-54e929095491.PNG)

- Configure MySQL server to allow connections from remote hosts:

![projectserver10a](https://user-images.githubusercontent.com/40290711/127070660-9ea7e24c-73cc-424c-bfa9-2cbc21c97e27.PNG)

- On the file replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:

![projectserver10](https://user-images.githubusercontent.com/40290711/127070889-676d0bf1-2261-4a2d-95b5-f78a54f102cb.png)

- Proceed to restart the MySQL server:

![projectserver10](https://user-images.githubusercontent.com/40290711/127071190-f0f110f4-e352-48a9-98e8-f3e6ac36a719.png)

- From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. Please use the mysql utility to perform this action: 

sudo mysql -u namesofuser -h privateIPaddress -p

![image](https://user-images.githubusercontent.com/40290711/127071468-253975c9-5acb-4a43-aa0f-7dfa3f1b7f00.png)

- Check that it has successfully connected to a remote MySQL server and can perform SQL queries:

SHOW DATABASE;

![projectserver13](https://user-images.githubusercontent.com/40290711/127071627-f8d544f2-6991-4bfa-8dfc-8c2a3e57fd71.PNG)

######## Congratulation you just finished this lap in flying color. 
