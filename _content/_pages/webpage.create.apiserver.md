### [![Index](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/home.png "Index") Index](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite)

## Create a basic api server

**Initialize the application**

Initialize your application with the relevant info.<br>
[How to initialize...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.initialize.md)

**How to add a web server to the application**

Now that we have a working node application , we can start adding more packages to the application to make it the application type we desire. We will be creating a web application , so we need a web server to handle HTTP requests.


For the web server we will use `express`.


For the NPM package details you can visit the site:

> https://www.npmjs.com/package/express


In the project root folder run the following command  : 

`> npm install express`

This will install the express package so we can use it.


**Adding express web server to your index file**


Open your `index.js` file and change the code to be the following:
```
const express = require('express')
const app = express()
 
app.get('/', function (req, res) {
  res.send('Hello World')
})
 
app.listen(3100)
```

Save the file and run the application.

[How to run...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.running.md)

The console output will still be 'hello world' but the application does not end.<br/>
It sits in a wait status listening on port 3100 for any HTTP traffic.

Open your browser and navigate to the following page: HTTP://localhost:3100/

The page will return 'hello world' as well.<br>
But this time it is the express server that sends a response back to the browser.

![Examples and lessons](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/footer.png "Examples and lessons")



