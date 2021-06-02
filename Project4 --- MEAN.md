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
+ Copy and paste the code below into routes.js:

![image](https://user-images.githubusercontent.com/40290711/120470390-606c7580-c39b-11eb-8246-7d8e8b5f9315.png)

+ In the ‘apps’ folder, create a folder named models:
- mkdir models && cd models
+ Create a file named book.js:
- vi book.js
+ Copy and paste the code below into ‘book.js’:

![project43](https://user-images.githubusercontent.com/40290711/120470898-fdc7a980-c39b-11eb-8e0b-cb763a6d5aca.png)

# Step 4 - Access the routes with AngularJS
- AngularJS is a JavaScript-based open-source front-end web framework mainly maintained by Google and by a community of individuals and corporations to address many of the challenges encountered in developing single-page applications.
+ Change the directory back to ‘Books’:
- cd ../..
+ Create a folder named public:
- mkdir public && cd public
+ Add a file named script.js:
- vi script.js
+ Copy and paste the Code below (controller configuration defined) into the script.js file:

![project44](https://user-images.githubusercontent.com/40290711/120473831-59476680-c39f-11eb-8e11-821c6c24b75d.png)

+ In ‘public’ folder, create a file named index.html
- vi index.html
+ Cpoy and paste the code below into index.html file:

![project45](https://user-images.githubusercontent.com/40290711/120474145-c0651b00-c39f-11eb-9903-a1fa2d1d8a03.png)


![project45b](https://user-images.githubusercontent.com/40290711/120474262-e7235180-c39f-11eb-9cf8-8f5e180737cd.png)

+ Change the directory back up to ‘Books’: 
- cd ..
+ Start the server by running this command:
- node server.js

![project46](https://user-images.githubusercontent.com/40290711/120475705-9f9dc500-c3a1-11eb-8bf1-6ca7acaf9a16.png)

+ We need to open TCP port 3300 in the AWS Web Console for the EC2 Instance:

![image](https://user-images.githubusercontent.com/40290711/120475420-3f0e8800-c3a1-11eb-9a3b-99cf7ef5883f.png)

+ Now you can access our Book Register web application from the Internet with a browser using Public IP address or Public DNS name:
- Public IP address:3300

![project47](https://user-images.githubusercontent.com/40290711/120475828-c2c87480-c3a1-11eb-8f38-84a3d688ea92.png)

![project48](https://user-images.githubusercontent.com/40290711/120475890-d8d63500-c3a1-11eb-8242-9510d6d227b4.png)

# Congrations, you just deployed your first MEAN stack application.

