---
title: Build a Todo App with Node.Js, ExpressJs, MongoDB and VueJs - Part 3
date: 2018-02-22 10:07:00 Z
categories:
- Node.js
tags:
- expressjs
- mongodb
- node-js
- vuejs
- docker
- docker-compose
layout: post
author: Samuel
comments: true
---

If you have made it to [Part 1](https://samuelabiodun.com/build-a-todo-app-with-node-js-expressjs-mongodb-and-vuejs-part-1/) and [Part 2](https://samuelabiodun.com/build-a-todo-app-with-node-js-expressjs-mongodb-and-vuejs-part-2/) of this tutorial, you deserve some accolades. And If you have not, you need to check [1](https://samuelabiodun.com/build-a-todo-app-with-node-js-expressjs-mongodb-and-vuejs-part-1/) and [2](https://samuelabiodun.com/build-a-todo-app-with-node-js-expressjs-mongodb-and-vuejs-part-2/) out. 

Let's take a moment to reflect on what we went through and how container technologies like docker could make this process easier.

In the previous tutorials, we installed Node runtime and MongoDB. The installation process was tedious and required a lot of manual efforts.

 Not only that, If Bill, who has a different environment, tries to run the application, chances are the application would not run correctly.  This happens most of the time because of inconsistency in environments and disparity in system libraries.  Using virtualization technologies like docker ensures that your applications run the same in all environments. 

This abolishes rooms for a famous saying, "It works on my machine." With docker, you can maintain a consistent environment everywhere.

### What is docker?

[Docker](https://www.docker.com/why-docker) is a container platform that allows you to seamlessly build, share, and run applications in any environment. With docker, you can define instructions on how your application should be built using DockerFile.

### Installing docker  and docker-compose tool

Docker is very easy to install. Irrespective of your operating system, you can find a distribution that works for your type of OS. Head on to [docker install page](https://docs.docker.com/install/) to download an executable docker file for your operating system.


Since we'll be using creating multiple containers for this application, you'll need to install [docker-compose tool](https://docs.docker.com/compose/install/) – a tool that lets you define and run multi-container Docker applications. Proceed to the official download page to download one for your operating system.

### Creating Dockerfile for backend Service

A `Dockerfile`  is a text file that contains a set of instructions. Docker read these instructions to build an image.

 Navigate to the backend directory and create a Dockerfile.

```bash
$ cd backend
$ touch Dockerfile


```

Update the content of the `Dockerfile` with this content.

```dockerfile
# backend/Dockerfile

FROM node:12.2.0-alpine 

WORKDIR /usr/app

COPY package*.json ./

RUN npm install

COPY . . 

CMD [ "npm", "start" ]
```

In this `Dockerfile`, we  used 5 different docker commands. 

- `FROM` allows you to specify a base image for your image. In this case, we based from `node:12.2.0-alphine`
- `WORKDIR`  allows you to set the current working directory. We set the current working directory  to `usr/app`.
- `COPY`:  This command let you copy files, directories or remote contents into the image file system.  We copied to our `package.json` and `package.lock.json`   to the docker image
- `RUN` : This command is used to execute command in the shell.  `RUN` command creates a new layer.  We use `RUN` to execute `npm install`  which installs our project dependencies.
- `COPY`: We used copy to copy our source code in to the container again
- `CMD` lets you set command to be executed when the container runs.

Let's proceed to create another `Dockerfile` in the frontend directory for the frontend. The commands will be simillar to the one we just created.

```bash
$ cd  frontend
```

```bash
$ touch Dockerfile
```

Update the `Dockerfile` you created with:

```dockerfile
#frontend/Dockerfile

FROM node:12.2.0-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

CMD ["npm", "run", "serve"]
```

Finally, we create a docker-compose file at the root directory of the project.

```yaml
version: "3"
services:
  backend:
    container_name: backend 
    build:
    	context: ./backend
    depends_on:
      - db  
    volumes:
      - ./backend:/usr/app
      - /usr/app/node_modules
    environment:  
      - MONGO_URL=mongodb://db:27017/todos
      - APP_PORT=80
    ports: ['80:80'] 
    
  db:
    container_name: db
    image: mongo:4.0
    restart: always
    
  frontend:
    container_name: frontend
    build:
      context: ./frontend
    volumes:
      - ./frontend:/app
      - /app/node_modules
    ports:
      - '8080:8080'
    environment:  
      - BACKEND_URL=http://localhost/todos
```



In the `docker-compose file,` we defined all our services.

The backend container is built from the instructions defined in `Dockerfile` in the backend directory. 

We used `depends_on` to specify dependencies for the container.

We used `volume` to share data between our local and host machine.

This is ideal for a development environment because we want to see our changes as we make them. You should not do that in production.

We defined 2 environment variables `APP_PORT`, `MONGO_URL,` and finally port 80 in the container to port 80 on our environment.


Finally, execute `docker-compose up -d ` to bring up your containers.

If you go to `http://localhost:8080`, you see  your app running.

You find the source code for this app on [Github](https://github.com/abiodunjames/NodeJs-Todo-List)

**Some useful links**

* [Docker Build: A Beginner’s Guide to Building Docker Images](https://samuelabiodun.com/Docker-Build-A-Beginners-Guide-to-Building-Docker-Images/)

* [Why Docker? Creating a multi-container application with docker](https://samuelabiodun.com/docker-why-and-creating-your-first-multi-container-application/)

  




