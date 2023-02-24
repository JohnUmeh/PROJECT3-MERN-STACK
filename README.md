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

## **Setting up Application code**

First, we create a Todo directory where all packages of our application would be contained.

`mkdir Todo`

Next, we change directory into the Todo directory created with `cd Todo` command 

After this, we run the `npm init` command. This enables a package.json file to be created which contains files that would enable our application and its dependencies to run









