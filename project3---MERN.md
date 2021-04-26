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
