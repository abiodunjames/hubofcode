---
title: Build a Todo App with Node.Js, ExpressJs, MongoDB and VueJs – Part 2
date: 2018-02-06 12:16:28 Z
categories:
- Node.js
tags:
- mongodb
- node-js
- vuejs
- expressjs
layout: post
author: Samuel
comments: true
---

![](https://res.cloudinary.com/samueljames/image/upload/v1570890498/frontendtest.gif)

In [part 1](https://samuelabiodun.com/build-a-todo-app-with-node-js-expressjs-mongodb-and-vuejs-part-1/) of this tutorial, we built APIs for a simple todo application and now we are here to put the front end together with VueJS. Dont worry if you are new to VueJs. I wrote [VueJs: The basics in 4 mins](https://codeburst.io/vuejs-the-basics-in-4-mins-6208df76003d) and [Creating your first component in VueJs](https://codeburst.io/building-your-very-first-component-with-vuejs-21b4a2f6a15a) to help you pick up VueJs in no time.

#### Project Directories

In part 1, we created `backend` directory. The `backend` directory contains the source code for our backend code.  

We'll do something simillar here. Let's create a new directory with the name `frontend`. This will house our frontend code.

```
$ mkdir frontend
```

If you run the command above, your project directory should now look like this:

```
.
├── backend
└── frontend
```

All our code in this post will go in to `frontend` directory.  

#### Vue CLI

[Vue CLI](https://cli.vuejs.org/guide/) is a command line tool that helps you quickly scaffold a new project. To install Vue CLI, run the command below from your terminal:

```bash
$ npm install -g @vue/cli
```

With the Vue Cli installed,  go to `frontend` directory run `vue create .`  from the command to scaffold a new project. 


```bash
$ vue create .
```

Ensure you answer *yes* to all prompts.

```
Vue CLI v3.5.1
? Generate project in current directory? Yes

? Please pick a preset: default (babel, eslint)
```

If everything went fine,  your frontend directory will look this:

```
├── README.md
├── babel.config.js
├── node_modules
├── package-lock.json
├── package.json
├── public
│   ├── favicon.ico
│   └── index.html
└── src
    ├── App.vue
    ├── assets
    │   └── logo.png
    ├── components
    │   └── HelloWorld.vue
    └── main.js


```

### Project Dependencies

1. [Bootstrap Vue]([https://bootstrap-vue.js.org](https://bootstrap-vue.js.org/)) : A  Vue compatible boostrap framework
2. [Sass loader](https://github.com/webpack-contrib/sass-loader): Compiles sass to css
3. [Axios](https://github.com/axios/axios) : For making rest API calls to todo API

Install bootstrap-vue and axis with the command:

```
$ npm install vue bootstrap-vue bootstrap axios
```

Install sass-loader with the command:

```
$ npm install sass-loader node-sass --save-dev

```

In the following paragraph, we'll create components we need for this project.

### Creating the Vue Components

Basically, we need 2 major vue components. The first component will be  `CreateTodo` and the second will be `ListTodo`

At some points, these components would need to communicate or share data with each other and this where event bus comes into play. 

One of the ways to handle communications between components in Vue.Js is to use a global event bus such that when a component emits an event, an event bus transmits this event to other listening components.

#### Event Bus

We create a global event bus with the name `src/bus.js`  with the code:

```javascript
//src/bus.js

import Vue from 'vue';

const bus = new Vue();
export default bus;

```

Now that we have created an event bus, let write the code for adding new todo items.

#### Vue Component for adding new todo items 

Create a new file at `src/components/CreateTodo.vue` and update its content with:
```vue

<template>
  <div class="col align-self-center">
    <h3 class="pb-5 text-left underline">Create todos</h3>
    <form class="sign-in" @submit.prevent>
      <div class="form-group todo__row">
        <input
          type="text"
          class="form-control"
          @keypress="typing=true"
          placeholder="What do you want to do?"
          v-model="name"
          @keyup.enter="addTodo($event)"
        />
        <small class="form-text text-muted" v-show="typing">Hit enter to save</small>
      </div>
    </form>
  </div>
</template>
<script>
import axios from "axios";
import bus from "./../bus.js";

export default {
  data() {
    return {
      name: "",
      typing: false
    };
  },
  methods: {
    addTodo(event) {
      if (event) event.preventDefault();
      let todo = {
        name: this.name,
        done: false //false by default
      };
      console.log(todo);
      this.$http
        .post("/", todo)
        .then(response => {
          this.clearTodo();
          this.refreshTodo();
          this.typing = false;
        })
        .catch(error => {
          console.log(error);
        });
    },

    clearTodo() {
      this.name = "";
    },

    refreshTodo() {
      bus.$emit("refreshTodo");
    }
  }
};
</script>
<style lang="scss" scoped>
.underline {
  text-decoration: underline;
}
</style>

```
* `addTodo()` is executed once an `enter` key is pressed. It makes a `POST` request to the backend with the new todo item.
* `clearTodo()` clears the input box once the todo item is saved.
* `refreshTodo()` emits an event `refreshTodo`. This is useful when you add a new todo item. It makes sense to re-render the list so that the new item is displayed.

That explained, let's go ahead to create `ListTodo` component.

####  Component for listing todo items

Create a file  `src/components/ListTodo.vue`  with the code:

```vue
<template>
  <div v-bind:show="todos.length>0" class="col align-self-center">
    <div class="form-row align-items-center" v-for="todo in todos">
      <div class="col-auto my-1">
        <div class="input-group mb-3 todo__row">
          <div class="input-group-prepend">
            <span class="input-group-text">
              <input
                type="checkbox"
                v-model="todo.done"
                :checked="todo.done"
                :value="todo.done"
                v-on:change="updateTodo(todo)"
                title="Mark as done?"
              />
            </span>
          </div>
          <input
            type="text"
            class="form-control"
            :class="todo.done?'todo__done':''"
            v-model="todo.name"
            @keypress="todo.editing=true"
            @keyup.enter="updateTodo(todo)"
          />
          <div class="input-group-append">
            <div class="input-group-text">
              <span
                class="input-group-addon addon-left"
                title="Delete todo?"
                v-on:click="deleteTodo(todo._id)"
              >
                X
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div
      class="alert alert-primary todo__row"
      v-show="todos.length==0 && doneLoading"
    >Hardest worker in the room. No more todos now you can rest. ;)</div>
  </div>
</template>

<script>
import axios from "axios";
import bus from "./../bus.js";

export default {
  data() {
    return {
      todos: [],
      doneLoading: false
    };
  },
  created: function() {
    this.fetchTodo();
    this.listenToEvents();
  },
  watch: {
    $route: function() {
      let self = this;
      self.doneLoading = false;
      self.fetchData().then(function() {
        self.doneLoading = true;
      });
    }
  },
  methods: {
    fetchTodo() {
      this.$http.get("/").then(response => {
        this.todos = response.data;
      });
    },

    updateTodo(todo) {
      let id = todo._id;
      this.$http
        .put(`/${id}`, todo)
        .then(response => {
          console.log(response);
        })
        .catch(error => {
          console.log(error);
        });
    },

    deleteTodo(id) {
      this.$http.delete(`/${id}`).then(response => {
        this.fetchTodo();
      });
    },

    listenToEvents() {
      bus.$on("refreshTodo", $event => {
        this.fetchTodo(); //update todo
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.todo__done {
  text-decoration: line-through !important;
}

.no_border_left_right {
  border-left: 0px;
  border-right: 0px;
}

.flat_form {
  border-radius: 0px;
}

.mrb-10 {
  margin-bottom: 10px;
}

.addon-left {
  background-color: none !important;
  border-left: 0px !important;
  cursor: pointer !important;
}

.addon-right {
  background-color: none !important;
  border-right: 0px !important;
}
</style>

```
 Let's take a moment to explain what is going on in the code.

We created 4 functions in the snippet.

* ` fetchTodo()` makes a `GET` call to backend and gets all todo items.
*  `updateTodo(todo)` is called when you make changes to todo items and hit enter. It forwards your changes to the backend.
* `deleteTodo(id)` runs when you click the trash button. It makes `DELETE` requests to the backend.
* `listenToEvents()`: In `CreateTodo` component, we emit events when a new todo item is added so the list. `ListTodo` is responsible for rendering todo items. This method does the job of listening for `refreshTodo` event.

####  App Component

Below we wrap all our components in a parent component named `App.vue`. Update the file `src/App.vue` with this content:

```js
<template>
  <div class="container">
    <div class="row vertical-centre justify-content-center mt-50">
      <div class="col-md-6 mx-auto">
        <CreateTodo></CreateTodo>
        <ListTodo></ListTodo>
      </div>
    </div>
  </div>
</template>

<script>
import CreateTodo from "./components/CreateTodo.vue";
import ListTodo from "./components/ListTodo.vue";

export default {
  name: "app",
  components: { CreateTodo, ListTodo }
};
</script>

<style lang="scss">
@import "node_modules/bootstrap/scss/bootstrap";
@import "node_modules/bootstrap-vue/src/index.scss";
.vertical-centre {
  min-height: 100%;
  min-height: 100vh;
  display: flex;
  align-items: center;
}
.todo__row {
  width: 400px;
}
</style>

```
#### Root Instance

A root instance must be defined for every vue application. You can see a Vue instance or root instance as the root of the tree of components that make up our app.

Let's modify the content of `src/main.js` file with:

```javascript
import Vue from 'vue';
import BootstrapVue from 'bootstrap-vue';
import axios from 'axios';
import App from './App.vue';

const http = axios.create({
  baseURL: process.env.BACKEND_URL ? process.env.BACKEND_URL : 'http://localhost/todos',
});

Vue.prototype.$http = http;

Vue.use(BootstrapVue);

Vue.config.productionTip = false;

new Vue({
  render: (h) => h(App),
}).$mount('#app');

```

We imported BoostrapVue and other libraries needed by the application.
 We also imported `App`  component and defined it as a component on the root instance. 

We imported `axios`, an http client and we configured the base url of the backend application.  You should ensure the `baseUrl` matches your backend URL.


We've come this far, run the application with:

```bash
$ npm run serve
```

It could take a few moment to build. At the end, you should have  a URL printend in the console:

```bash
App running at:
  - Local:   http://localhost:8080/ 
  - Network: http://192.168.178.20:8080/
  Note that the development build is not optimized.
  To create a production build, run npm run build.
```

If you navigate to `http://localhost:8080`, you should be greeted with a page like this.
![Todo App](https://res.cloudinary.com/samueljames/image/upload/v1570888955/Screenshot_2019-10-12_at_16.01.32.png)



To connect the frontend app with the backend, you also need to start the backend server.

Navigate to `backend` directory and run

```bash
$ npm run start

```
#### Note:

1. Your MongoDB connection URL must be properly configure in `backend/config/Config.js` and the MongoDB must be running.
2. Your backend server must be running.
3. Your frontend server must be running.


If you navigate to http://localhost:8080, you will be greeted with a page like this.![](https://res.cloudinary.com/samueljames/image/upload/v1570890498/frontendtest.gif)

[Get the source code here](https://github.com/abiodunjames/NodeJs-Todo-List)

