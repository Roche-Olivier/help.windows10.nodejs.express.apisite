### [![Index](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/home.png "Index") Index](https://github.com/Roche-Olivier/help.windows10.nodejs.express.website)

## Implemetation of packages


**Initialize the application**

Initialize your application with the relevant info.<br>
[How to initialize...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.initialize.md)

**Add a web server to the application**

Add a `express` web server to your application <br>
[How to add express...](https://github.com/Roche-Olivier/help.windows10.nodejs.express.website/blob/master/_content/_pages/webpage.create.webserver.md)


**Add the relevant libraries to your header**

Add the relevant libraries to your index.html page <br>
[How to add libraries to the header...](https://github.com/Roche-Olivier/help.windows10.nodejs.express.website/blob/master/_content/_pages/webpage.create.webserver.packages.md)

**Using the libraries**

Now that we have the libraries in our header, they will load whenever the page loads. We can utilize them to create almost anything on the page.<br>
Lets take alook at a very basic example.

***

**jQuery**

> https://jquery.com/

> jQuery is a fast, small, and feature-rich JavaScript library. It makes things like HTML document traversal and manipulation, event handling, animation, and Ajax much simpler with an easy-to-use API that works across a multitude of browsers. With a combination of versatility and extensibility, jQuery has changed the way that millions of people write JavaScript.

Lets take a look at a very basic example<br>
We want to monitor when the DOM is ready , and then display a meesage that the docuet is rady:

Open you index.html file and change the scrpt part to the following:
```
    <script>
        $(document).ready(function () {
            console.log("ready!");
            console.log("This is the console log")
        });
    </script>
```
Save the files and run the application.

[How to run...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.running.md)

Open your browser and navigate to the following page: HTTP://localhost:3100/

***


**Bootstrap**

> https://getbootstrap.com/docs/4.0/getting-started/introduction/

> Get started with Bootstrap, the world’s most popular framework for building responsive, mobile-first sites, with BootstrapCDN and a template starter page.

One of the libraries is `Bootstrap`, lets take a look how that works. Open your `index.html` file and change the body to look like this:
```
<body>
    <div class="container">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">Card title</h5>
                <p class="card-text">
                    <div class="jumbotron">
                        Hello to my static page...
                    </div>
                </p>
                <a href="#" class="btn btn-primary">Go somewhere</a>
            </div>
        </div>
    </div>

    <script>
        $(document).ready(function () {
            console.log("ready!");
            console.log("This is the console log")
        });
    </script>
</body>
```
Save the files and run the application.

[How to run...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.running.md)

Open your browser and navigate to the following page: HTTP://localhost:3100/

The page of the site would look like this now.

![WFE_with_bootstrap](https://github.com/Roche-Olivier/help.windows10.nodejs.express.website/blob/master/_content/_images/screen/wfe_with_bootstrap.png "WFE_with_bootstrap")






***


**Knockout**

> https://knockoutjs.com/documentation/introduction.html

> Knockout is a JavaScript library that helps you to create rich, responsive display and editor user interfaces with a clean underlying data model. Any time you have sections of UI that update dynamically (e.g., changing depending on the user’s actions or when an external data source changes), KO can help you implement it more simply and maintainably.

> Developers familiar with Ruby on Rails, ASP.NET MVC, or other MV* technologies may see MVVM as a real-time form of MVC with declarative syntax. In another sense, you can think of KO as a general way to make UIs for editing JSON data… whatever works for you :)

Lets see how this works:
Change your body of your `index.html` page to look like this<br>
```
<body>
    <div class="container">
        <div class="card">
            <div class="card-body">
                <h5 class="card-title">Card title</h5>
                <p class="card-text">
                    <div class="jumbotron">
                        Hello to my static page...
                        <span data-bind="text: my_observable_field"></span>
                    </div>
                </p>
                <a href="#" class="btn btn-primary">Go somewhere</a>
            </div>
        </div>
    </div>

    <script>
        var dm
        $(document).ready(function () {
            console.log("ready!");
            console.log("This is the console log")

            dm = new DocumentModel(this);
            page_load.init_ko()
            ko.applyBindings(dm);

            dm.my_observable_field("My value")
        });

        function DocumentModel(hostThisContext) {
            var self = this;
            self.hostContext = hostThisContext;
        }
        var page_load = {
            init_ko: function () {
                dm.my_observable_field = ko.observable()
            }
        }
    </script>
</body>
```

Save the files and run the application.

[How to run...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.running.md)

Open your browser and navigate to the following page: HTTP://localhost:3100/



***


**moment**

> https://momentjs.com/docs/

> Moment was designed to work both in the browser and in Node.js.<br> All code should work in both of these environments, and all unit tests are run in both of these environments.<br> Currently the following browsers are used for the ci system: Chrome on Windows XP, IE 8, 9, and 10 on Windows 7, IE 11 on Windows 10, latest Firefox on Linux, and latest Safari on OSX 10.8 and 10.11.<br> If you want to try the sample codes below, just open your browser's console and enter them.

Lets see how this works:
Change your scripts section of your `index.html` page to look like this<br>
```
    <script>
        var dm
        $(document).ready(function () {
            console.log("ready!");
            console.log("This is the console log")

            dm = new DocumentModel(this);
            page_load.init_ko()
            ko.applyBindings(dm);

            var now  = moment(new Date()).format("ddd YYYY-MM-DD");
            dm.my_observable_field(now)
        });

        function DocumentModel(hostThisContext) {
            var self = this;
            self.hostContext = hostThisContext;
        }
        var page_load = {
            init_ko: function () {
                dm.my_observable_field = ko.observable()
            }
        }
    </script>
```



Save the files and run the application.

[How to run...](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_pages/start.running.md)

Open your browser and navigate to the following page: HTTP://localhost:3100/

![Examples and lessons](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/footer.png "Examples and lessons")



