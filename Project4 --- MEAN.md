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

