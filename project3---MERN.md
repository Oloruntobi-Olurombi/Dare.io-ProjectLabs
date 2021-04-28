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
- Paste the code below into it then save and exit:
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;

-Setting up MongoDB
What is MongoDB: MongoDB is a source-available cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with optional schemas. 
- logged on to https://www.mongodb.com and create an amount 
- create an organisation, also create a project, create a user and choose the Atlas and create a cluster.
- Click on Network Access and allow access from anywhere
- Create a Database and also create a collection inside of the database.
- Create a .env file : touch .env
- Paste : DB = mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority (please don't add ' ' to the string connection in the DB variable)
- Ensure to update <username>, <password>, <network-address> and <database> according to your setup
- Update the index.js to reflect the use of .env so that Node.js can connect to the database:
- const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
- Start server using the command:
node index.js
You shall see a message ‘Database connected successfully’

- Testing Backend Code without Frontend using RESTful API
- For this task I will using Insonmia App instead of Postman (I find Insonmia easy to use)
- Create a create Todo Endpoint:
- 
