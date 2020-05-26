### [![Index](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/home.png "Index") Index](https://github.com/Roche-Olivier/help.windows10.nodejs.express.website)

## Useful packages and libraries

**Initialize the application**

Initialize your application with the relevant info.<br>
[How to initialize...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.initialize.md)

**Add a web server to the application**

Add a `express` web server to your application <br>
[How to add express...](https://github.com/Roche-Olivier/help.windows10.nodejs.express.website/blob/master/_content/_pages/webpage.create.webserver.md)


**Relevant packages to add**

* jQuery - DOM elements and page
* Bootstrap - Used for styling the applications
* Knockout - binding observables
* moment - anything time related

For the purpose of this tutorial i am going to use the CDN references to the libraries...

**jQuery:**
```
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js" integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo=" crossorigin="anonymous"></script>
```
**Bootstrap**
```
<--CSS-->
<link href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">

<--SCRIPT-->
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
```

**Knockout**
```
<script src="https://cdnjs.cloudflare.com/ajax/libs/knockout/3.5.0/knockout-min.js" integrity="sha256-Tjl7WVgF1hgGMgUKZZfzmxOrtoSf8qltZ9wMujjGNQk=" crossorigin="anonymous"></script>
```

**moment**
```
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.24.0/moment.min.js" integrity="sha256-4iQZ6BVL4qNKlQ27TExEhBN1HFPvAvAMbFavKKosSWQ=" crossorigin="anonymous"></script>
```

Add all those libraries to your `head` of the `index.html` file.
To follow standards and have the page render we need to add some more header information, for example the title , the page icon, etc. You can add any icon to your images folder and then reference it in the header as your page icon.<br>


Lets add the reference to our css file as well. We can use this later to override styles we want to personalize.

Create a file called `app_base.css` in your `content > css` folder.
 

When we have added all the relevant files etc, our `index.html` page should look like this

```
<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="icon" type="image/png" href="/images/favicon.png" sizes="36x36">
    <title>Hello world</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"
        integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo=" crossorigin="anonymous"></script>
    <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"
        integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM"
        crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/knockout/3.5.0/knockout-min.js"
        integrity="sha256-Tjl7WVgF1hgGMgUKZZfzmxOrtoSf8qltZ9wMujjGNQk=" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.24.0/moment.min.js"
        integrity="sha256-4iQZ6BVL4qNKlQ27TExEhBN1HFPvAvAMbFavKKosSWQ=" crossorigin="anonymous"></script>
    <script type="text/javascript" src="/js/app_base.js"></script>
    <link rel="stylesheet" href="/css/app_base.css">
</head>

<body>
    Hello to my static page...
    <script>
        console.log("This is the console log")
    </script>
</body>

</html>
```


Save the files and run the application.

[How to run...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.running.md)

Open your browser and navigate to the following page: HTTP://localhost:3100/

![Examples and lessons](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/footer.png "Examples and lessons")



