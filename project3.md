# Documentaion of Project 3

## Step 1: Backend Configuration
- Update ubuntu via `sudo apt update`
- Upgrade ubuntu via `sudo apt upgrade`
- Get the location of Node.js software from Ubuntu repositories, using `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`
- Install Node.js with `sudo apt-get install -y nodejs`
- Verify the node installation with `node -v`
- Verify the node installation with `npm -v`
![nodejs-installed](./images/nodejs-installed.PNG)
- Create new directory for To-Do project, `mkdir Todo`
- To verify that the Todo directory is created with ls command, run `ls`
- Change current directory to newly created one via `cd Todo`
- Use the command `npm init` to initialise project, so that a new file named package.json will be created. This file will normally contain information about the application and the dependencies that it needs to run. Follow the prompts after running the command. Press Enter several times to accept default values, then accept to write out the package.json file by typing yes.
- Run the `ls` to confirm that you have package.json file created.
![package-json-1](./images/package-json-1.PNG)
### Install ExpressJS
- Install Express via `npm install express`
![package-json-1](./images/package-json-1.PNG)
- Now create a file index.js with `touch index.js`
- Run `ls` to confirm that index.js file is successfully created
- Install the dotenv module via `npm install dotenv`
- Open the index.js file with `vim index.js`
- Type in the code below and save.

const express = require('express');
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
- Use `:w` to save in vim and use `:qa` to exit vim
- Open terminal in the same directory as your index.js file and type `node index.js`
![server-running](./images/server-running.PNG)
- Create an inbound rule to open TCP port 5000 in EC2 Security Groups
![port-5000](./images/port-5000.PNG)
- Open up browser and try to access server’s Public IP or Public DNS name followed by port 5000: http://< PublicIP-or-PublicDNS >:5000
![welcome-to-express](./images/welcome-to-express.PNG)
- Create a folder called "routes" via `mkdir routes`
- Change directory to "routes" folder, using `cd routes`
- Now, create a file api.js with `touch api.js`
- Open the file with `vim api.js`
- Copy below code in the file.

const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;

### Models
- To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier. Change directory back Todo folder with `cd ..` and install Mongoose via `npm install mongoose`
- Create a new folder called "models" via `mkdir models`
- Change directory into the newly created "models" folder with `cd models`
- Inside the models folder, create a file and name it todo.js: `touch todo.js`
- (Tip: All three commands above can be defined in one line to be executed consequently with help of && operator, like this: `mkdir models && cd models && touch todo.js`)
- Open the file created with `vim todo.js` and paste the code:

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

- In "routes" directory, open api.js with `vim api.js`, delete the code inside with `:%d` command and paste there code below into it then save and exit

### MongoDB Database
- Create a MongoDB database and collection inside mLab
![mongodb](./images/mongodb.PNG)
- Create a file in Todo directory named .env, using `touch .env && vi .env`
- Add connection string to access the database in it: `DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`
- Update index.js to reflect the use of .env so that Node.js can connect to the database.
![index-js](./images/index-js.PNG)
- Start server via `node index.js`
- ![database-connected](./images/database-connected.PNG)
- 

DB = 'mongodb+srv://Adesuwa:gracious7@d-aze-cluster.7plud.mongodb.net/myFirstDatabase?retryWrites=true&w=majority'





http://54.89.95.7:5000/api/todos

## Step 2: Frontend Creation
- In Todo directory, run: `npx create-react-app client`
![npx](./images/npx.PNG)
- Install Concurrently (used to run more than one command simultaneously from the same terminal window) via `npm install concurrently --save-dev`
![concurrently](./images/concurrently.PNG)
- Install Nodemon (used to run and monitor changes the server code) via `npm install nodemon --save-dev`
![nodemon](./images/nodemon.PNG)
- In Todo folder open the package.json file and edit.
![package-json](./images/package-json.PNG)
### Configure Proxy
- Had to find a way to update Node as requested the first time I did the `npx create-react-app client` step, and redo the step
![node-fix-1](./images/node-fix-1.PNG)
![node-fix-2](./images/node-fix-2.PNG)
![node-fix-3](./images/node-fix-3.PNG)
![node-fix-4](./images/node-fix-4.PNG)
- Change directory to ‘client’: `cd client`
- Open the package.json file via `vi package.json`
- Add the key value pair in the package.json file `"proxy": "http://localhost:5000"`
