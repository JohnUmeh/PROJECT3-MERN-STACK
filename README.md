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

Use :w to save in vim and use :qa to exit vim

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



















