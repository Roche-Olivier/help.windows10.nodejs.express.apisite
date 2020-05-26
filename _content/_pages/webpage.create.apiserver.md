### [![Index](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/home.png "Index") Index](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite)

## Create a basic api server


**Use the WFE template for a quick start to your API site**

> <a href="https://github.com/Roche-Olivier/Examples/tree/master/Examples/NodeJs/WFE_Template__2019_06"><b>WFE template 2019-06</b></span></a> 

After you have the files locally, navigate you this application root directory and open a prompt.<br>
You can run the following command to install all the packages in the package.json file<br>

`> npm install`

***



**Creating a folder structure for a API application**

Create a folder structure that looks like this under your main application folder.<br>
``` 
+-- application_folder
|   +-- api
|      +-- bll
|         +-- app_bll.js
|      +-- controllers
|         +-- app_controller.js
|      +-- infrastructure
|         +-- httphandler.js
|      +-- routes
|         +-- app_route.js
|   +-- content
|      +-- css
|         +-- app_base.css
|         +-- bootstrap.min.css
|      +-- images
|      +-- js
|         +-- pages
|            +-- app_intialize.js
|         +-- app_base.js
|         +-- bootstrap.min.js
|         +-- jquery-3.4.1.min.js
|         +-- knockout-min.js
|         +-- moment.min.js
|      +-- pages
|         +-- index.html
|   +-- node_modules
|   +-- swagger
|      +-- swagger.json
|   +-- index.js
|   +-- package-lock.json
|   +-- package.json
|   +-- README.md
```

> Create the files and the folders , some of the files and folders will be used in the later tutorial, but while we are here lets do it, then its done.

If you started with the WFE template you will only need to add the `api` and `swagger` folder part

We do this to keep the web pages that is served , away from the routes of the API , and the server away from both. This splits the application in functional parts for the application that is grouped together.


***

**Update the home page**

Update the home page to show a nice UI to a developer that would like to use your API's.<br>
You can change your index.html file under the pages folder to look like this.
```
<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Hello world</title>
    <script type="text/javascript" src="/js/jquery-3.4.1.min.js"></script>
    <link rel="stylesheet" href="/css/bootstrap.min.css">
    <script type="text/javascript" src="/js/bootstrap.min.js"></script>
    <script type="text/javascript" src="/js/knockout-min.js"></script>
    <script type="text/javascript" src="/js/moment.min.js"></script>
    <script type="text/javascript" src="/js/app_base.js"></script>
    <link rel="stylesheet" href="/css/app_base.css">
    <script type="text/javascript" src="/js/pages/app_intialize.js"></script>

</head>

<body>
    <div class="container">
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
            <a class="navbar-brand" href="pages/index.html">Home</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav"
                aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav">
                    <li class="nav-item">
                        <a class="nav-link" href="/swagger">Open API documentation</a>
                    </li>
                </ul>
            </div>
        </nav>
    </div>
    <div class="container">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">Welcome to the API site </h5>
                <p class="card-text">
                    <div class="jumbotron">
                        API - template<br>
                    </div>
                </p>
            </div>
        </div>
    </div>
</body>

</html>
```


We now have a structure for an API site with a UI to greet the developer when they access the API site.

![Examples and lessons](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite/blob/master/_content/_images/footer.png "Examples and lessons")



