### Adding a server

Running NG Serve runs a server which servers the front-end. It's not a server you can use for the backend to work though. 
There are 2 ways of connecting an angular backend. The first is tohave a node app that serves the fronte-end as part of it. The alternative is having a node express server for the business logic and a seperate static server for the Angular frontend.
- Node App serves Angular SPA. Node handles incoming requests, requests targeting '/' return the Angular SPA.
- Two Seperate servers. Node handles incoming requests. Angular SPA seperated from the static host. I both cases we get logically seperated apps. Angular handles the UI and sends requests and node handles the requests and serves the logic.

## What is a Restful api

Representational state transfer (REST) is a server side solution that serves data  to a frontend or other service app. It will store and serve data when that data is requested. A restful api is a stateless backend. It exposes different paths that then return data to whatever requested that data. Different paths may support different request types (CRUD paths). Express tends to send json data back to the frontend.

## Example in node

```
const http = require('http') // default node js package

const server = http.createServer((req, res) => { // server creates a server
  res.end('THis is my first reponse')
})

server.listen(process.env.PORT || 3000) // use the global set port, or 3000 if that env isn't set

```

## Example in express

```
// server.js
const http = require('http') // default node js package
const app = require('./backend/app')

const port = process.env.PORT || 3000

app.set('port', port) // set the port on the app
const server = http.createServer(app) // the express app is handling all the interactions

server.listen(port) // use the global set port, or 3000 if that env isn't set

//  app.js
const express = require('express') // express is a node module

const app = express(); // an express app is just a big chain/funnel of middleware that funnels ecery request made to the endpoints
// we can manipulate the things that happen in this funnel

app.use((req, res, next) => {
  console.log('First')
  next() // next() allows the request to continue
}) // this sets up a new middleware on the request


app.use((req, res, next) => {
  res.send('hello from express') // this will send a reponse to whatever requested the info
})

module.exports = app;

```



