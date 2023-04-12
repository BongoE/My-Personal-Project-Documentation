In this project, we would build a book register Application that has frontend using Angular Framework and a backend Express that stores Information on a Database (Mongo DB) and this is running on a Node JS Javascript enviroment on a browser.
We shall use the ## OSI Model (Open Source Interconnect). There are 7 layers in this model.
# MEAN Stack is a combination of following components:
- MongoDB (Document database) – Stores and allows to retrieve data.
- Express (Back-end application framework) – Makes requests to Database for Reads and Writes.
- Angular (Front-end application framework) – Handles Client and Server Requests
- Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user.  **Runtime environment is a sub-system that exists both in the computer where a program is created, as well as in the computers where the program is intended to be run**
- ## Task
- Our task is to implement a simple Book Register web form using MEAN stack.
- 
# Step 0 – Preparing prerequisites
In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.
I created an Ubuntu t2-micro instance on the AWS management console.

# Step 1: Install NodeJs
Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express
routes and AngularJS controllers.

Update ubuntu. It is always important to update, so as to avoid any incompatibility.Versions are constantly being updated and upgraded. I updated and upgraded my Instance 

```
sudo apt update
```

Upgrade ubuntu

```
sudo apt upgrade
```

Add certificates

```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
```

Install NodeJS

```
sudo apt install -y nodejs **-y** is to skip the yes prompting and update everything. ``node --version`` to check the uploaded version
```

# Step 2: Install MongoDB
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can
be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author,
and number of pages.

Follow the steps  from this page:  https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/

mages/WebConsole.gif. To Install Mongo DB we first of all need to add the key to our Key server then add the repository for Mongo DB

```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

```
sudo apt-get update
Reload local package database 
```

Install MongoDB 

```
sudo apt-get install -y mongodb-org
```

Start The server

```
sudo systemctl start mongod
```

Verify that the service is up and running

```
sudo systemctl status mongod (NOT mongodb)
```
We need some dependencies to make our Porgram work effectively. To install these packages, we would require the services of the Node Package Manager (npm)

NPM is also a repository manager
Install npm – Node package manager.

```
sudo apt install -y npm
```

Install body-parser package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.


```
sudo npm install body-parser
```
We have installed body-parser. We need a root directory where we shall build our application
Create a folder named ‘Books’

```
mkdir Books && cd Books (This command both creates a directory and cd into it)
```

In the Books directory, Initialize npm project

```
npm init (prompt isn't returned! We need to give insome values of the npm)
```

Add a file to it named server.js

```
vi server.js
```

Copy and paste the web server code below into the server.js file.

```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
```
cat server.js to view content. Viewed. Expected outcome visible
# Step 3: Install Express and set up routes to the server
Express is a backend Application that makes read and write requests to Database. It would be used to pass book requests to and from our MongoDB Database
We would use ''Mongoose'' packages which provides a straight forward schema -based solution to model our application package
Make sure to run the package in the Books directory.
Run ls to see the installed packages.
In Books folder, create a directory called apps and cd into it
```
mkdir apps && cd apps
```
Create a file called **route.js**
```
vi route.js 
```
copy this code block into the route.js
```
var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};

```
Use the cat route.js to ensure hat what you pasted aligns with the documentation. Ensured!

In the app folder, create another folder called models and the cd into the folder
```
mkdir models && cd models
```
Create a file in the models folder called book.js
```
vi book.js
```

copy and paste the code below in the newly created file books.js
```
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);

```
# Step 4: Access the routes with AngularJS
Angular is a front end Framework which handels client-side and server-side requests.
We use Angular to connect our web page with Express and perform actions on our book register

We need to go back to the 'Books` directory
```
cd ../..
```
Create a folder named public

```
mkdir public && cd public
```
Add a file named script.js
```
vi script.js
```
Copy and paste the Code below
```
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});

```
In the public folder, we need to create another file called index.html. This is for the front end code
```
vi indec.html
```
We need to cd back into the route directory and run the  node server.js
```
cd ..
```
Start the server by running this command:
```
node server.js
```
