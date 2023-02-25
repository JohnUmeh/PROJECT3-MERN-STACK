#  **PROJECT3-MERN-STACK**
IMPLIMENTING A WEB SOLUTION BASED ON MERN STACK ON AWS WEB SERVER

## **MERN**

MERN stack is a web development framework. It consists of MongoDB, ExpressJS, ReactJS, and NodeJS as its working components. These components are used in developing a web application when using MERN stack as follows:

   **MongoDB**: A document-oriented, No-SQL database used to store the application data.

   **NodeJS**: The JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

   **ExpressJS**: A framework layered on top of NodeJS, used to build the backend of a site using NodeJS functions and structures.

   **ReactJS**: A library created by Facebook. It is used to build UI components that create the user interface of the single page web application
    
         
 ## **Creating an Ec2 instance**
    
   We start the project by creating an ec2 instance on aws server and saving the keypair with type .pem
    
   ![Screenshot from 2023-02-24 04-48-40](https://user-images.githubusercontent.com/77943759/221087378-3cb7b049-73e9-429b-81a8-3aa408328bc0.png)
    
   After this, on your windows terminal, cd into the directory containing your .pem file and connect to the instance by running:
    
  `ssh -i <private_keyfile.pem> username@ip-address`
    

 # **BACKEND CONFIGURATION**
  
  First, we run the commands `sudo apt update` and `sudo apt upgrade`
  
  ![apt update](https://user-images.githubusercontent.com/77943759/221088385-ca352cc2-adb6-450b-85eb-712364227f6a.png)

Next, we find the location of node.js in ubuntu repositories with this command

`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

![nodecurl](https://user-images.githubusercontent.com/77943759/221088924-0fa267aa-9f3d-4685-8fab-89f0e30e85dc.png)

Now, we use `sudo apt-get install -y nodejs` command to install node.js on our machine. With this, we will install both node and npm which is a package manager for nodejs used to install other modules and manage dependency conflict

![nodeinstall](https://user-images.githubusercontent.com/77943759/221089108-3cbff5f6-1eb4-468b-ae15-97b4c4a3fd18.png)

Run `node --v` to verify node was installed and `npm --v` to verify npm was installed

![node and npmv](https://user-images.githubusercontent.com/77943759/221094449-a985cc11-9076-40a1-8469-87f17a2aae7b.png)


## **Setting up Application code**

First, we create a Todo directory where all packages of our application would be contained.

`mkdir Todo`

Next, we change directory into the Todo directory created with `cd Todo` command 

After this, we run the `npm init` command. This enables a package.json file to be created which contains files that would enable our application and its dependencies to run.

![todo init](https://user-images.githubusercontent.com/77943759/221094294-ecbeef63-0628-482f-b739-4974d867165c.png)

## **Install Expressjs**

Next, we install the javascript express js which we will help us in URL routing and HTTP request making in our application

Run `npm install express` to install expressjs 

![installexpress](https://user-images.githubusercontent.com/77943759/221096160-ab47bd2e-175e-446a-ac5d-c6ce19260724.png)


Create index.js file by running 

`touch index.js`

Next, we install the dotenv module, this a package that helps keep passwords, API keys, and other sensitive data out of code. It allows us to create environment variables in a .env file instead of putting them in our code.

`npm install dotenv`

![dotenv](https://user-images.githubusercontent.com/77943759/221104401-5b18794f-7d28-48ac-a93a-bc43677c5372.png)


Open index.js file using the command

`vim index.js`

copy and paste the in index.js with the following code:

```const express = require('express');
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
```

Use `esc`, :wq to save and exit vim

Next, we test if the server is working correctly by running the command `node index.js`

![node500confirm](https://user-images.githubusercontent.com/77943759/221105099-6f6020be-fd0f-4fdc-917e-962fa06719de.png)

The image shows that our server is running on port 5000 and we need to open this port 5000 in our ec2 instance security group settings inbound rule

![tcp5000](https://user-images.githubusercontent.com/77943759/221105933-bf025a90-b596-4ee2-88d7-e73031e521d3.png)

Now need to access the server through our public ip address or DNS with this command

`http://<PublicIP-or-PublicDNS>:5000`

![welcexpressip](https://user-images.githubusercontent.com/77943759/221106506-2fe2fd39-9e71-4479-b30b-42a76126c258.png)

The image above shows we can access the server. Done

### **Creating Routes**

Our Todo app is expected to be able to create 

1. Create new task
2. Get list of all tasks
3. Delete completed tasks

For each of these, we need to create an endpoint and use standard HTTP request methods: POST; GET; DELETE

we need to create routes that will define various endpoints that the To-do app will depend on for each task. Let us create routes:

`mkdir routes`

change directory to routes 

`cd routes`

Create api.js file with

`touch api.js`

open the file with `vim api.js` 

Copy and paste the following line of codes

```const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
```
![editapijs](https://user-images.githubusercontent.com/77943759/221110875-74d383fc-ad8b-4a6c-b3f1-713b3ae8ea9e.png)

Save and exit with `esc`, :wq and hit enter

### **Installing Models**

Since we are using mongodb for this application, we create models for it. Models makes javascript based applications interactive. 

To create this, we will install mongoose; 

Change directory to the Todo directory with `cd ..`

Type in command `npm install mongoose`

![mongoose](https://user-images.githubusercontent.com/77943759/221333044-efc89ff3-3dce-4080-bb70-aa7b258e76b0.png)


Next, create models directory with `mkdir models`

change directory into models with `cd models` and create a named todo.js inside with `touch todo.js`

Open file todo.js with `vim todo.js` and paste the following line of codes within:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```

![todojsedit](https://user-images.githubusercontent.com/77943759/221333246-a01046da-b87b-4624-8512-14cb5364f15a.png)

Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with vim api.js, delete the code inside with :%d command and paste the code below into it then save and exit

```
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
```

![editapijs](https://user-images.githubusercontent.com/77943759/221333344-a7c7a306-f2dd-46e7-ac2e-a724237c20de.png)

# **INSTALL DATABASE MONGODB**

We need a database for the storage of our data from the application and for this purpose, we will be using mongodb. Visist https://www.mongodb.com/atlas-signup-from-mlab to create an account. Select shared cluster and aws as the cloud provider, choose the closest region to you.

Login into mLab and create a cluster

![mongodbreg](https://user-images.githubusercontent.com/77943759/221335342-07816b8c-c5b6-48cb-890e-fc6aec0a243b.png)

Create a database and a collection 

![dbcollection](https://user-images.githubusercontent.com/77943759/221337450-2e363312-8a94-460b-a204-e42f32215c32.png)


Connect mongoose to our database service: We connect to it using the `connect` details from mongodb

Click on clusters, then click on connect

![connstring](https://user-images.githubusercontent.com/77943759/221338146-74c416f3-04a5-40ac-afa0-3eed75af9be8.png)

![constring2](https://user-images.githubusercontent.com/77943759/221338182-ff31ca97-28d3-4dc1-9ccc-133b2c139cb9.png)


Copy the connection code and save it inside a `.env` file which should be created in the parent `todo` directory

`DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`

Change the username, password,database name and network address to the one specified by you when creating the database and collection.

Update the code in our `index.js` as we need to point mongoose to the database service we created using mLab

```
const express = require('express');
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
console.log(`Server running on port ${port}`)![postman](https://user-images.githubusercontent.com/77943759/221338746-4bfe1493-c5ff-46b1-bf01-26fd5de44a20.png)

});
```
Start the server by running command `node index.js`

![connectnodejs](https://user-images.githubusercontent.com/77943759/221338356-85871f9d-f5a2-4d28-b031-e5466e2b4881.png)

The message of connecting successfully above confirms our server runs perfectly. 

## **Testing Backend Code using RESTful API**

In this case, we will use Postman, an API development client to test our code since we dont have frontend UI yet

Visit `https://www.postman.com/downloads/` to download and istall Postman

Open postman and create a `POST` request with the endpoint `http://<PublicIP-or-PublicDNS>:5000/api/todos`, in the header section: Select `content-Type` as key and application/json as value; in the body session: select raw and enter any message of your choice.


![postrequest](https://user-images.githubusercontent.com/77943759/221338768-e8fa60fa-308c-473f-ba4b-75fc4fbb012b.png)

Next, create a `GET` request with same endpoint `http://<PublicIP-or-PublicDNS>:5000/api/todos`

![getrequest](https://user-images.githubusercontent.com/77943759/221338999-6e9f83c0-9736-4861-a7d9-e68660574108.png)

Create a `DELETE` request 

![deleterequest](https://user-images.githubusercontent.com/77943759/221339073-3811b6c7-7328-4219-a30b-c3b087981016.png)

We have been able to:

Create task (POST request)

Display available tasks (GET request)

Remove/Delete completes tasks (DELETE request) 

# **CREATING FRONTEND**

We need to create a user interface for clients to interact with the server

We will use `create-react-app client` command to create a new directory in our Todo directory

Run `npx create-react-app client` in the Todo directory

We need to install the following dependencies before testing our react app

**concurrently**: It is used to run more than one command simultaneously from the same terminal window.

`npm install concurrently --save-dev`

**nodemon**: It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

`npm install nodemon --save-dev`

    Go to Todo directory,  open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.
```    
 "scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```    
    
 ![wheretochange](https://user-images.githubusercontent.com/77943759/221339809-2f1d33dc-f9d0-4122-9956-6e98c35cc4d8.png)
 
 ## **Configure Proxy in package.json**


Change directory to client `cd client`

Open package.json file `vi package.json`

Add `"proxy": "http://localhost:5000"` keypair to the file. This makes it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos.

Go to Todo directory and run `npm run dev`

![npmrundev](https://user-images.githubusercontent.com/77943759/221340111-376134ea-4179-4f09-bd56-b801e084d875.png)

Our server is now running on port 3000. 

We need to open this port in our ec2 instance inbound rule setting

![tcp3000](https://user-images.githubusercontent.com/77943759/221340184-a66f1cdd-7fc4-4c24-b13f-6517621ca1d2.png)

## **Creating your React Components**

React makes use of components, this makes codes modular and the components are reusable. We will create 2 stateful and 1 stateless component for our Todo app

`cd client` 

`cd src`

We will create a directory for components in src 

`mkdir components`

We will create these three new files in components directory: Input.js, ListTodo.js, and Todo.js

`touch Input.js ListTodo.js Todo.js`

open Input.js file with `vi Input.js` and paste the code below inside

```import React, { Component } from 'react';
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
```

To make use of Axios, which is a Promise based HTTP client for the browser and node.js, we need to cd into into client from the terminal and run `npm install axios`

`cd ..`

`cd ..`

Run:

`npm install axios`

![axios](https://user-images.githubusercontent.com/77943759/221341054-240d2865-8003-418d-8067-9711d58a865f.png)

Go back to the component directory `cd src/components`

Open ListTodo file `vi ListTodo.js` and paste the code below inside

```import React from 'react';

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
```
![ListTodo](https://user-images.githubusercontent.com/77943759/221341787-c8857d42-fee2-4e7a-9ffc-0854ae3c782e.png)


Open Todo.js `vi Todo.js` and paste the following code inside

```import React, {Component} from 'react';
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
```
![Todojs](https://user-images.githubusercontent.com/77943759/221341738-c68b3248-62ad-479c-b15a-ae19c5806ab6.png)


We need to make adjustment to our react code. Delete the logo and adjust our App.js. 

Return to the src directory

`cd ..`

In the src folder, open App.js `vi App.js` and paste the code below

```import React from 'react';

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
```
![appjs](https://user-images.githubusercontent.com/77943759/221341671-80337841-4621-440f-b197-16be046bf144.png)

Save and exit.

In the src directory, open index.css file `vim index.css`

Copy and paste this:

```
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
```

![indexcssedit](https://user-images.githubusercontent.com/77943759/221342152-813d4768-92e9-4149-bf50-11b8b908a910.png)


Return to Todo directory `cd ../..`

Run `npm run dev`

![myapp](https://user-images.githubusercontent.com/77943759/221342508-3d068bbd-1676-4e14-9d4e-f8a2093acb60.png)

This page confirms we have successfully created a Todo app

END



















