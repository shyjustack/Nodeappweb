# Dockerizing a Node.js web app

The purpose of this document is to show you how to get a Node.js application into a Docker container
# Create the Node.js app
First, create a new directory where all the files would live. In this directory create a package.json file that describes your app and its dependencies:

```
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
```

With your new package.json file, run npm install. If you are using npm version 5 or later, this will generate a package-lock.json file which will be copied to your Docker image.

Then, create a server.js file that defines a web app using the Express.js framework:
```
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
  res.send('Welcome to our page!');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```
In the next steps, we'll look at how you can run this app inside a Docker container using the official Docker image. First, you'll need to build a Docker image of your app.

Creating a Dockerfile
Create an empty file called Dockerfile:
```
touch Dockerfile
```
Open the Dockerfile in your favorite text editor

```
FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD ["node", "server.js"]
```

# Building your image

```
docker build . -t <your username>/node-web-app
```
Your image will now be listed by Docker:
```
docker images
```
# Run the image
Running your image with -d runs the container in detached mode, leaving the container running in the background. The -p flag redirects a public port to a private port inside the container. Run the image you previously built

```
docker run -p 49160:8080 -d <your username>/node-web-app
```

# Testing 
To test your app, get the port of your app that Docker mapped:
In the example above, Docker mapped the 8080 port inside of the container to the port 49160 on your machine.

Now you can call your app using curl (install if needed via: sudo apt-get install curl):

```
curl -i localhost:49160
```

We hope this tutorial helped you get up and running a simple Node.js application on Docker.

<h3 align="left">Connect with Me:</h3>
<a href="https://linkedin.com/in/shyjustack" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="cloudnloud" height="30" width="40" /></a>
