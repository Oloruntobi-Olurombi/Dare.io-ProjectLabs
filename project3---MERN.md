#WEB STACK IMPLEMENTATION (MERN STACK) IN AWS 
LEMP STACK is a technology stack that stands for MongoDB, Express, ReactJS and NodeJS. 
Steps in deploying a MERN STACK using AWS EC2 instance and MobaXterm:
- Step up AWS account and an EC2 instance with an ubuntu server
- Open a new session with MobaXterm
-  Update Ubuntu : sudo apt update
-  Upgrade Ubuntu : sudo apt upgrade
-  Get the location of Node.js software from Ubuntu repositories : curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
-  Install Node.js on the server : sudo apt-get install -y nodejs
-  Check Node Version : node -v
-  Verify the node installation  : npm -v 
Application Code Setup
- Create a new directory : mkdir Todo
- Change current directory to the newly created one : cd Todo
- Initialise the project using : npm init
- Install ExpressJS : npm install express
- Create a file index.js : touch index.js
- Install the dotenv module : npm install dotenv
- Open the index.js file: vim index.js : write some code
- const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});

- Save and quit : :wq!

-Start server : node index.js
- Add a new inbound rule in EC2 Security Groups @TPC 5000
- Open up browser and try to access server’s Public IP using :  http://<PublicIP-or-PublicDNS>:5000
![newproject3a](https://user-images.githubusercontent.com/40290711/116162814-7167fe00-a6ee-11eb-9647-18625a285192.PNG)

- Create routes
-There are three actions that our To-Do application needs to be able to do: a) Create a new task, b) Display list of all tasks and c) Delete a completed task
- Create a folder routes : mkdir routes
- Change directory to routes folder : cd routes
- Create a file api.js : touch api.js
- Open the file with the command : vim api.js
