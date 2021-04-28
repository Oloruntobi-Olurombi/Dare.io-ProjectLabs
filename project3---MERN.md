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
- For this task I used Insonmia App instead of Postman (I find Insonmia easy to use)
- Create a Post to the API Endpoint:
![newproject3b](https://user-images.githubusercontent.com/40290711/116369466-50dc9880-a801-11eb-82e1-ec490c6df676.PNG)

- Create a Get to the API Endpoint:
![newproject3c](https://user-images.githubusercontent.com/40290711/116370388-42db4780-a802-11eb-92b0-703ae640b2e5.PNG)

- Create a Delete to the API Endpoint:
![newproject3d](https://user-images.githubusercontent.com/40290711/116370710-9188e180-a802-11eb-903a-648da053af89.PNG)

- Set Up Frontend 
- Use the create-react-app command to scaffold our app in the root directory : $ npx create-react-app client
- This will create a new folder in the Todo directory called client, add all the react code in the client directory

- Running a React App : Before testing the react app, there are some dependencies that need to be installed
npm install concurrently --save-dev && $ npm install nodemon --save-dev

-In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below:
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},

- Configure Proxy in package.json

- Change directory to ‘client’
cd client

Open the package.json file

vi package.json
Add the key value pair in the package.json file "proxy": "http://localhost:5000"

- The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos
- Ensure you are inside the Todo directory, and simply do : npm run dev
- App should open and start running on localhost:3000 : Please add an inbound rule in the EC2 instance Security Group (TCP, Allow access from everywhere, Port: 3000)
- Creating React Components
- From your Todo directory run : cd client
- Move to the src directory : cd src
- Inside your src folder create another folder called components
- mkdir components
- Move into the components directory with : cd components

Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.
touch Input.js ListTodo.js Todo.js

Open Input.js file

- vi Input.js
Copy and paste the following

import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input

- install Axios to fetch data from the backend
- Move to the src folder : cd ..
- Move to clients folder : cd ..

- Install Axios : $ npm install axios

- Go to ‘components’ directory : cd src/components
After that open your ListTodo.js

- vi ListTodo.js
in the ListTodo.js copy and paste the following code:

import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo

- Then in the Todo.js file  write the following code

import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;

- Need to make little adjustment to the react code. Delete the logo and adjust our App.js to look like this.

- Move to the src folder : cd ..
- Make sure that you are in the src folder and run : vi App.js
Copy and paste the code below into it

import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;

After pasting, exit the editor : :wq!

- In the src directory open the App.css : vi App.css
Then paste the following code into App.css:

.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}

Exit : :wq!

- In the src directory open the index.css : vim index.css

Copy and paste the code below:

body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}

- Go to the Todo directory : cd ../..
When in the Todo directory run : npm run dev

- Assuming no errors when saving all these files, the To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all the tasks:

![newproject3e](https://user-images.githubusercontent.com/40290711/116377342-fd6e4880-a808-11eb-94b3-3b94b0cbc01f.PNG)
![newproject3f](https://user-images.githubusercontent.com/40290711/116377356-0232fc80-a809-11eb-97e7-c1029e643e76.PNG)

