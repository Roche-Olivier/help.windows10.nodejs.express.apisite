### [![Index](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/home.png "Index") Index](https://github.com/Roche-Olivier/help.windows10.nodejs.express.website)

## Serve pages with your web server


**Initialize the application**

Initialize your application with the relevant info.<br>
[How to initialize...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.initialize.md)

**Add a web server to the application**

Add a `express` web server to your application <br>
[How to add express...](https://github.com/Roche-Olivier/help.windows10.nodejs.express.website/blob/master/_content/_pages/webpage.create.webserver.md)


**Adding 'path'**

We need to add the `path` package to be able to navigate the folders.

For the NPM package details you can visit the site:

> https://www.npmjs.com/package/path

In the project root folder run the following command :

`> npm install path`

We now have the path package to work with in our application. Lets create the folder structure next to keep the server separate from the client code.


**Creating a folder structure for a WFE application**

Create a folder structure that looks like this under your main application folder.<br>
``` 
+-- application_folder
|   +-- content
|      +-- css
|      +-- images
|      +-- js
|         +-- app_base.js
|      +-- pages
|         +-- index.html
|   +-- node_modules
|   +-- index.js
|   +-- package-lock.json
|   +-- package.json
|   +-- README.md
```

> Create the files and the folders , some of the files and folders will be there already


**Setting up the server to find the HTML pages**

Change your `index.js` file to look like this.
```
const express = require('express')
const path = require('path')
const app = express()

app.use('/css/', express.static(path.join(__dirname, '/content/css')));
app.use('/images/', express.static(path.join(__dirname, '/content/images')));
app.use('/js/', express.static(path.join(__dirname, '/content/js')));

app.get('/', function (req, res) { res.sendFile(path.join(__dirname + '/content/pages/index.html')); });
app.get('/home/', function (req, res) { res.sendFile(path.join(__dirname + '/content/pages/index.html')); });

app.listen(3100)
```

`__dirname` is the folder the current app is executing in.

This shows the express server where all the files is that you will need to render the web page.

In other words it resolves all the web paths to paths on the file system.


**Adding content to the HTML page**

Open the `index.html` file under the pages folder and add the following code to the page.<br>
```
<html>
<head>
    <script type="text/javascript" src="/js/app_base.js"></script>
</head>
<body>
    Hello to my static page...
    <script>
        console.log("This is the console log")
    </script>
</body>
</html>
```

We now have a static HTML page with a reference to a js file and some script part added to it. This is not being used , just to show where things will come.


Save the files and run the application.

[How to run...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.running.md)

Open your browser and navigate to the following page: HTTP://localhost:3100/

![Examples and lessons](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/footer.png "Examples and lessons")



