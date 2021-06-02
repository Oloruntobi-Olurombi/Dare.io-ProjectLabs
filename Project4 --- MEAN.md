## MEAN STACK IMPLEMENTATION IN AWS EC2 instance and Ubuntu server. 

MEAN is a technology stack that stands for MongoDB, Express, Angular and NodeJS.

Steps in deploying a MEAN STACK using AWS EC2 instance running on an Ubuntu server with MobaXterm as IDE and SSH channel:

- Step up AWS account and an EC2 instance with an ubuntu server
- Open a new session with MobaXterm using SSH

# Step 1 : Install Node.js
- Update Ubuntu : sudo apt update
- Upgrade Ubuntu : sudo apt upgrade
- sudo apt install -y nodejs

# Step 2: Install MongoDB
- MongoDB is a source-available cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with optional schemas.
- Paste the following code:
- sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
- echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
- sudo apt install -y mongodb
+ Start the server using the following:
- sudo service mongodb start
+ Verify that the service is up and running:
- sudo systemctl status mongodb
![project41](https://user-images.githubusercontent.com/40290711/120465841-5005cc00-c396-11eb-82fb-c06a19d39eca.png)
- When you see the above it shows that the server is running successfully.
- Press Ctrl + C to Esc on a Windows System.
+ Install Node Package Manager
- sudo apt install -y npm
+ Install Body-Parser
- sudo npm install body-parser
- ‘body-parser’ package helps us process JSON files passed in requests to the server.

# Application Code Setup
+ Create a folder named ‘Books’:
- mkdir Books && cd Books
+ In the Books directory, Initialize npm project:
- npm init
+ Add a file to it named server.js
- vi server.js
+ Copy and paste the web server code below into the server.js file:
![project42](https://user-images.githubusercontent.com/40290711/120468147-cefc0400-c398-11eb-8d09-98786ffff01b.png)

# Step 3: Install Express and set up routes to the server
- Express.js, or simply Express, is a back end web application framework for Node.js, released as free and open-source software under the MIT License. It is designed for building web applications and APIs. It has been called the de facto standard server framework for Node.js.
- We also will use Mongoose package which provides a straight-forward, schema-based solution to model our application data.
+ sudo npm install express mongoose
+ In ‘Books’ folder, create a folder named apps
- mkdir apps && cd apps
+ Create a file named routes.js
- vi routes.js
+ Copy and paste the code below into routes.js

![project43](https://user-images.githubusercontent.com/40290711/120469544-61e96e00-c39a-11eb-9ff3-952127cbd0b2.png)
