---
layout: post
title: Build a Todo App with Node.Js, ExpressJs, MongoDB and VueJs - Part 1
author: 'Samuel'
categories: Node.js
comments: true
date: '2018-01-20 16:07:00'
tags:
- expressjs
- mongodb
- node-js
- vuejs
---

![Hint: This is what the final result would look like.](https://res.cloudinary.com/samueljames/image/upload/v1570890498/frontendtest.gif)

In this tutorial, we will build the famous todo application with Node.Js using [the ExpressJs](https://expressjs.com/) framework and [MongoDB]([https://MongoDB.com]). Did I forget to tell you? The app will be API centric and full-stack :).  

 In a nutshell, if you want to learn how to build APIs with Node.Js, you have come to the right place. 

 Now you can grab a bottle of beer and let's get our hands dirty.

#### What is ExpressJs?

ExpressJs simply put, it's a web framework for Node.Js - *stolen from the official docs.* Taylor Otwell (Laravel's creator) once said, "Developers build tools for developers".  ExpressJs was built for developers to simplify Node APIs.  

#### What is MongoDB?

MongoDB is a NoSQL database. It's completely document-oriented. A NoSQL database allows you to store data in the form of JSON and any formats. If you want to learn more about MongoDB, I have also written a post on MongoDB [here](https://samuelabiodun.com/MongoDB-on-GCP-Setting-it-up-and-keeping-it-runnning/).

#### Defining Todo  APIs

I like to begin by defining my APIs. The table below shows what APIs we need to create and  what each does. 

| Method        | Path | Description  |
| ------------- |:-------------:| -----:|
| GET      | /todos | Get all  todos items |
| GET | /todos/:id | Get  one todo item |
| POST | /todos | Create a new todo item |
| PUT | /todos/:id |    Update a  todo item |
| DELETE | /todos/:id | Delete a new todo item |

Having defined our APIs, let's delve in to the project directories.

#### Project Directories

You are probably going to get a shock of your life when I tell ya we need not more than 5 files with relatively few lines of code to build this backend. Yes! It is that simple. This how we roll man :).

You should create a project folder at this point to house all the source code for the backend part of this application. I'll call my backend. Feel free to use the `backend` as your directory. It's not patented.:)

 To keep things simple, and of course, we should not throw away flexibility at the sight of simplicity. We'll create just 4 folders in the directory you created above.

1. *config*: This contains configuration files for the app.
2. *models*: This houses our entity (Todo data structure).
3. *repositories*: A repository adds an abstraction layer on the data access. You can read more about why it's important to have this layer [here](http://blog.sapiensworks.com/post/2014/06/02/The-Repository-Pattern-For-Dummies.aspx)
4. *routes*: A route is an entry to your application. This folder contains a file that defines what should happen when a user accesses a particular route.

```
├── config
├── models
├── respositories
└── routes
```

#### What you will need to install

1. You will need [Node](http://nodejs.org/download)
2. You will need to install [MongoDB](http://mongodb.org/) 

#### Application Packages

This app depends on a couple of packages and will use npm to install them. Navigate to the project directory you just created and create a _package.json_ file with the content below.

```json
{
    "name": "node-todo",
    "version": "1.0.0",
    "description": "Simple todo application.",
    "main": "server.js",
    "author": "Samuel James",
    "scripts": {
        "build": "webpack",
        "start": "node app.js"
    },
    "dependencies": {
        "cookie-parser": "~1.4.4",
        "cors": "^2.8.5",
        "debug": "~2.6.9",
        "express": "~4.16.1",
        "http-errors": "~1.6.3",
        "jade": "~1.11.0",
        "mongoose": "^5.7.3",
        "morgan": "~1.9.1"
    }
}

```

Run `npm install` to install the dependencies. Let's go ahead and define config parameters our app needs.

#### Config file

We'll define database connection URL and the port  the app will listen on in `config/Config.js` file as follows: 

```js
//config/Config.js

module.exports = {
  DB: process.env.MONGO_URL ? process.env.MONGO_URL : 'mongodb://localhost:27017/todos',
  APP_PORT: process.env.APP_PORT ? process.env.APP_PORT : 80,
};

```

In the `config/Config.js`,  we set `DB`  to environment variable `MONGO_URL` if defined, otherwise defaults to `mongodb://localhost:27017/todos`.  We also did the same with `APP_PORT`.

#### Todo Model

Model is an object representation of data in the database. So, Let's  create a file `models/Todo.js` with the content:

```javascript
//models/Todo.js

const mongoose = require('mongoose');

const { Schema } = mongoose;

// Define schema for todo items
const todoSchema = new Schema({
  name: {
    type: String,
  },
  done: {
    type: Boolean,
  },
});

const Todo = mongoose.model('Todo', todoSchema);

module.exports = Todo;

```

If you notice, we use mongoose for schema definition, right? Mongoose is an official MongoDB library built for manipulating MongoDB databases in Node. In the structure, I have defined `name` and `done`. 

_name_: This is the name of the todo item. We define it as string.  For example, "I'm going for swimming by 3pm"
_done_: Todo item status which is boolean. If the todo item is still pending, its value will be false.

Now, we'll create a todo repository.

#### Repositories

I like to see repository as a strategy for abstracting data access. I became a big fan when this pattern saved me from heavy refactoring when I switched to a new datastore in one of my  projects. It helps you decouple your project and reduces duplication in your code.  Here is an [interesting article]( https://makingloops.com/why-should-you-use-the-repository-pattern/) I recommend you read on this pattern.

That said, create a file _repositories/TodoRepository.js_ as:

```js
//repositories/TodoRepository

const Todo = require('../models/Todo');

class TodoRepository {
 
  constructor(model) {
    this.model = model;
  }

  // create a new todo
  create(name) {
    const newTodo = { name, done: false };
    const todo = new this.model(newTodo);

    return todo.save();
  }

  // return all todos
  
  findAll() {
    return this.model.find();
  }

  //find todo by the id
  findById(id) {
    return this.model.findById(id);
  }

	// delete todo
  deleteById(id) {
    return this.model.findByIdAndDelete(id);
  }

  //update todo
  updateById(id, object) {
    const query = { _id: id };
    return this.model.findOneAndUpdate(query, { $set: { name: object.name, done: object.done } });
  }
}

module.exports = new TodoRepository(Todo);

```

With `TodoRepository.js` defined, let's go create  todo routes.

#### Routes

Every web application has at least an entry point. A route in web apps is more like saying: "Hey Jackson when I ask ya for this, give me that". Same goes for our app, we'll define what URL users need to access to get certain results or trigger certain actions.

In this case, we want users to be able to perform create, read, update and delete (CRUD) operations on todo items. 

Now, that you know what "routes" does, create _routes/Routes.js_  file and the code below:

```js
const express = require('express');

const app = express.Router();
const repository = require('../respositories/TodoRepository');

// get all todo items in the db
app.get('/', (req, res) => {
  repository.findAll().then((todos) => {
    res.json(todos);
  }).catch((error) => console.log(error));
});

// add a todo item
app.post('/', (req, res) => {
  const { name } = req.body;
  repository.create(name).then((todo) => {
    res.json(todo);
  }).catch((error) => console.log(error));
});

// delete a todo item
app.delete('/:id', (req, res) => {
  const { id } = req.params;
  repository.deleteById(id).then((ok) => {
    console.log(ok);
    console.log(`Deleted record with id: ${id}`);
    res.status(200).json([]);
  }).catch((error) => console.log(error));
});

// update a todo item
app.put('/:id', (req, res) => {
  const { id } = req.params;
  const todo = { name: req.body.name, done: req.body.done };
  repository.updateById(id, todo)
    .then(res.status(200).json([]))
    .catch((error) => console.log(error));
});
module.exports = app;

```
First, you would want users to get a list of all to-do items existing in the database, hence we defined a route (_/all_) that accepts a get request and returns a JSON object of todo items if successful.

Our users like to get items as well as store new items, we added a route to create new to-do items. It accepts a post request.
When Mr. A makes a post request to route (_/add_), a new to-do item is created in the database.

Once a todo-item is completed, we also want users to be able to mark it as done. To do this, one must know what item a user intends to mark as done in the first place. So, we defined an 'update route' with a route parameter which is the ID of the item to update.

#### Server File

Having defined all routes that our application needs, it is time to create an entry file which is the main file to run our project.

At the project root folder, create a _app.js_ file and update its content with this:

```js
//app.js

const createError = require('http-errors');
const express = require('express');
const path = require('path');
const mongoose = require('mongoose');
const cookieParser = require('cookie-parser');
const logger = require('morgan');
const cors = require('cors')

const config = require('./config/Config');

const routes = require('./routes/Routes');

const app = express();

mongoose.connect(config.DB, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

app.use(cors());  //enable cors

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/todos', routes);

// catch 404 and forward to error handler
app.use((req, res, next) => {
  next(createError(404));
});

// error handler
app.use((err, req, res) => {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

app.listen(config.APP_PORT); // Listen on port defined in environment


module.exports = app;

```

We required:

* Express Js

* morgan - _a logging middleware for Node_

* path - a _package for working with file and directory_

* mongoose - _a package for working with MongoDB_

* body-parser - _a_ _body parsing middleware_

  We set our app to listen on the port set in _config/Config.js._  We also defined it to use routes defined in _routes/Routes.js_ and prefix with _todos_.

Finally,  your directory structure should look like this:

```
├── app.js
├── config
│   └── Config.js
├── models
│   └── Todo.js
├── package-lock.json
├── package.json
├── respositories
│   └── TodoRepository.js
└── routes
    └── Routes.js
```

To start the application, navigate to project root  and run:

```bas
npm start
```

Let's create a new todo item

```
$ curl -H "Content-Type: application/json" -X POST -d '{"name":"Going Shopping"}' http://localhost/todos

{"__v":0,"name":"Going Shopping","done":false,"_id":"5a6365a39a2e56bc54000003"}
```

Get all todo items

```
$ curl  http://localhost/todos

[{"_id":"5a6365a39a2e56bc54000003","name":"Doing Laundry","done":false,"__v":0},{"_id":"5a6366039a2e56bc54000004","name":"Going Shopping","done":false,"__v":0}]
```
[Get the source code here](https://github.com/abiodunjames/NodeJs-Todo-List)