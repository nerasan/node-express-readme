# Node ReadMe

[Node Project File](#creating-node-project-file)

[Express App File](#creating-express-app-file)

[Sequelize](#sequelize)

## What is Node and Express?

Node is a runtime environment for running JavaScript and it allows us to create applications where both the client and server use JavaScript. We will use a web framework called Express to create a secure web server that interacts with a database.

## Creating Node Project File

During this unit, we are creating a Node project file. In order to create this, we need to run the following in our Terminal within the directory we want.

```javascript
npm init
```

It will then ask to fill in settings and descriptions. Either fill them out or press 'enter' to leave it blank. Once it asks to confirm the settings, type 'yes'.

Then open the package.json file in your code editor. It will need an entry point to which you can just have "index.js" (or any other .js file, but make sure it is created).

To run a file using node, type the following in the Terminal (for example, to run the index.js file):

```javascript
node index.js
```

### Nodemon

Nodemon is a useful tool that allows the node application to automatically restart when file changes in the directory are detected. This means you will not have to continuously run "node index.js" to test every change. Note that this means while you are in the middle of writing code, Nodemon might give you errors because it identifies incomplete code. It is important to run Nodemon when testing your Node applications and opening the local host destination (localhost:8000 for example) otherwise it will give an error in the browser.

We can install Nodemon globally for all Node files to access by running the following in the Terminal.

```javascript
npm install -g nodemon
```

### .gitignore

As you add in more node modules, your "node_modules" folder will get bigger. To ensure we are not always pushing all the files within node_modules into your repo on GitHub, create a .gitignore file within the directory. Then add (in plain text) the files you would like for it to ignore. Eventually we will get a list of what is generally to be ignored as we use more node modules. This helps so your repo is not heavy/slow.

For now, we can put "node_modules" in the .gitignore file to ignore the whole folder from pushing. Note that the .gitignore file is a plain text file.

With this said, make sure your repository is a git repository by using "git init" in the directory.

Note: You can also create a .gitignore file in the root directory and add "node_modules" to it by running the following command in Terminal:
```javascript
echo "node_modules" >> .gitignore
```

## Creating Express App File

We are creating a new Express file. In order to create this, we need to run the following in our Terminal within the directory we want. (-y overrides the default descriptions/settings)
```javascript
npm init -y
```

After creating the node file, open the file in your code editor and then install Express using the following in the Terminal:
```javascript
npm i express
```

Make sure the entry point file is created too (the index.js). You will need to import the Express module into your index.js by adding the following in index.js and creating an instance of the express app:

```javascript
const express = require('express')
const app = express()
```

Create a home route using get:

```javascript
app.get('/', function(req, res) {
    res.send('hello, world!')
})
app.listen(8000)
```

Run nodemon and make sure the browser loads properly when you visit localhost:8000.

Note: If nodemon does not work and indicates port 8000 is being used elsewhere, kill all nodes by running the following and making sure nodemon is run again. Make sure you properly exited from the Terminal (iTerm).

```javascript
killall node
nodemon
```

### Response Object

There are more functions we can use in addition to res.send().

* res.send() - Sends back a simple string. Similar to console.log() where it is good for testing purposes.
* res.sendFile() - Sends an entire file back, but the file is static.
* res.render() - Renders data into templates with the selected template engine.
* res.json() - Sends object data back as JSON.

### More Route Styles

More examples of additional routes.

```javascript
app.get('/', (req, res) => {
  res.send('You've reached the home route!');
});

app.get('/about', (req, res) => {
  res.send('This is a practice app to learn about express routes.');
});
```

### Views

Create a "views" folder inside your project directory. This folder will store the HTML files that will make up the routes of your site. (This can also be.ejs files, which will be discussed later.)

Once you have basic HTML or however you want your route to look like, you will replace the "res.send()" function to have:
```javascript
app.get('/', (req, res) {
    res.sendFile(__dirname+'/views/index.html')
})
```

In this example, the index.html will correspond to the 'home page' where the route is just "/". You will have to use the res.sendFile() function and link the HTML file for all routes you have.

### Templates

While sending HTML files is nice, it is a static file. We can have customization on the page and be able to manipulate the page (think DOM manipulation). Template engines allow us to inject values into the HTML, and even script logic into the HTML. There are many JavaScript template engines for Express. One of the most popular is EJS.

First, we will need to add EJS to the website project using npm:
```javascript
npm i ejs
```

Then, above the routes, we need to set the view engine to EJS by adding the following. This tells Express that we will use EJS as our view engine.
```javascript
app.set('view engine', 'ejs');
```

Next, we will adapt the routes to EJS.

1. Rename the .html files to .ejs files.
2. Replace the "res.sendFile(<html path file>) statements with "res.render(<ejs file name>)" statements.
3. EJS assumes the path to the template files are nested in a "views" folder and have .ejs extensions, therefore you can just pass the file name without adding the path or .ejs extension. (optional)

For example, your home route should look like this:
```javascript
app.get('/', (req, res)=>{
  res.render('index.ejs');
});
```

### Templating with Variables

We can actually pass an object to res.render() and access the values stored in it as variables inside the .ejs template.

For example, we can create an obkect with one key-value pair and pass that object in as the second argument to the render function in one of your routes.
```javascript
app.get('/', function(req, res) {
  res.render('index', {name: "salima harun", age: 26});
});
```

We can access this variable by embedding it into the html using the following notation: <%= embedded js goes here %> 
```javascript
<!DOCTYPE html>
<html>
  <head>
    <title>Home Page</title>
  </head>
  <body>
    <h1>Hello, <%= name %>!</h1>
  </body>
</html>
```

Any JavaScript can be embedded using the <% %> tags. The addition of the = sign on the opening tag means that a value will be printed to the screen. Without the = sign, the embedded js within the tags will execute, not print out.

### Layouts

Express gives us a lot of flexibility but we need to be sure it is organized. We can utilize layouts and controllers in an Express app through EJS layouts. EJS layouts is a node package that allows us to create a boilerplate (or a template) that we can inject content into based on which route is reached. The layouts generally include a header and footer that is to be displayed on every page. For example, these could be the navigation bar, menu, logo, etc. that helps a website look consistent on every route.

First, we will need to install EJS layouts via npm.

```javascript
npm i express-ejs-layouts
```

Second, we set up EJS layouts to the app.
```javascript
var express = require('express');
var app = express();
var ejsLayouts = require('express-ejs-layouts');

app.set('view engine', 'ejs');
app.use(ejsLayouts);

app.listen(3000)
```

Next, we create a layout in the root of the "views" folder by creating "layout.ejs" file. It MUST be called layout.ejs as mandated by EJS layouts.
```javascript
<!DOCTYPE html>
<html>
<head>
  <title>sample webpage</title>
</head>
<body>
  <%- body %>
</body>
</html>
```

The layout will be used by all pages and the content will be filled in where the <%- body %> tag is placed. This is a special tag used by EJS layouts that cannot be renamed. In the above example code, the layout currently has only the title to say "sample webpage" for all pages.

Next, to use the layout, we will need to run the following. In the views folder, create a "home.ejs" file. Within the home.ejs file, you can add anything you want that will display in the body tag for the home route. (Example: A header stating this is the home page.)

Then, create a home route in index.js below the middleware:
```javascript
app.get('/', (req, res) => {
  res.render('home');
});
```

Continue setting up the views for all your other routes.

### .env (dotenv)

```javascript
touch .env
```

Initialize dotenv:
```javascript
npm i dotenv
```

Store environment variables in this file such as:
```javascript
API_KEY=XXXXXX
PORT=8000
```

Always add .env to your .gitignore.

## Sequelize

Sequelize is an ORM (Object-Relational-Mapper) that works with relational databases and Node.js. It allows us to: represent models and their data, represent associations between these models, validate data before they gets persisted to the database, and perform database operations in an object-oriented fashion.

### Sequelize Setup

Get the sequelize-cli tool (only have to do once).

```javascript
npm install -g sequelize-cli
```

Create a new folder, add an index.js and .gitignore, and initialize the repository.

```javascript
mkdir userapp
cd userapp
npm init -y
touch index.js
echo "node_modules" >> .gitignore
```
Add/save dependencies (sequelize needs pg for Postgres).
```javascript
npm install pg sequelize
```
Initialize a sequelize project.
```javascript
sequelize init
```

Update the config.json, models, and migration files to look like the following:

**config/config.json**
```javascript
  "development": {
    "database": "userapp_development",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "test": {
    "database": "userapp_test",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "production": {
    "database": "userapp_production",
    "host": "127.0.0.1",
    "dialect": "postgres"
  }
}
```
* change the dialect defaults from "mysql" to "postgres"
* change the database names (usually to what yoour directory is called)
* delete the username and password (not needed for mac users)

**Create database inside of Postgres**

```javascript
sequelize db:create userapp_development
```
**Create a model and a matching migration**

```javascript
sequelize model:create --name user --attributes firstName:string,lastName:string,age:integer,email:string
```
* Specify the name of the model to be singular using the --name flag (in this example we used *user*, so the name of the table will be *users*)
* After the --atributes flag, pass in data about your model.
* Generating the model also generates a corresponding migration. You only need to do this once for your model.
* Do not have spaces between each of the attributes and their data types.

**models/user.js (created after the above is run)**
```javascript
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class user extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      // define association here
    }
  };
  user.init({
    firstName: DataTypes.STRING,
    lastName: DataTypes.STRING,
    age: DataTypes.INTEGER,
    email: DataTypes.STRING
  }, {
    sequelize,
    modelName: 'user',
  });
  return user;
};
```
* If you want to make changes to your model after generating it, all you have to do is make a change in this file and save it before running the migrate command.

**Add a corresponding migration** 

migrations/*create-user.js
```javascript
'use strict';
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('users', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      firstName: {
        type: Sequelize.STRING
      },
      lastName: {
        type: Sequelize.STRING
      },
      age: {
        type: Sequelize.INTEGER
      },
      email: {
        type: Sequelize.STRING
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('users');
  }
};
```

**Running the migration**

```javascript
sequelize db:migrate
```

If you need to undo the last migration, this command will undo the last migration that was applied to the database.

```javascript
sequelize db:migrate:undo
```
Use the psql shell to verify that your database and table was created.
```javascript
psql
\l
\c userapp_development
\dt
```

### Using Models

Remember just like the other modules, your models must be required in order to access them in your app.
```javascript
const db = require('./models')
```

### CRUD with Sequelize

**Create**
```javascript
db.user.create({
    firstName: 'Taylor',
    lastName: 'Darneille',
    age: 27
}).then(createdUser=>{
    // the create promise returns the
    // new row of data that has been created
    // (otherwise it throws an error)
    console.log(createdUser)
})
```

**Read One**
```javascript
db.user.findOne({
    where: {firstName: 'Taylor'}
}).then(foundUser=>{
    console.log(foundUser)
})
```

**Find or Create**
```javascript
db.user.findOrCreate({
  where: {
    firstName: 'Brian',
    lastName: 'Smith'
  },
  defaults: { age: 88 }
}).then(([user, created])=>{
  console.log(user); // returns info about the user
});
```

**Find All**
```javascript
db.user.findAll().then(users=>{
  console.log(users);
  // users will be an array of all User instances
});
```

**Update**
```javascript
db.user.update({
    lastName: 'Taco'
  }, {
    where: {
      firstName: 'Brian'
    }
}).then(numRowsChanged=>{
    console.log(numRowsChanged)
});
```

**Destroy**
```javascript
db.user.destroy({
    where: { firstName: 'Brian' }
  }).then(numRowsDeleted=>{
      console.log(numRowsDeleted)
    // do something when done deleting
  });

```

### Other Notes on Sequelize
* After a sequelize statement, we can interact with the return of that object by using .then.
* .then - default promise called when a query is completed.
* .catch - triggered if something goes wrong/error.
* .finally - triggered after all other callbacks. Can be used for cleanup.
