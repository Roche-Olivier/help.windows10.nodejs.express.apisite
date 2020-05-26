### [![Index](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/home.png "Index") Index](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite)

## Server routing with parameters


Open your site with the basic routing already in it.<br>
[API server : Adding routing](https://github.com/Roche-Olivier/Examples/wiki/API-server-:-Adding-routing)


***

Nothing changes in your index.js file if you stay within the same route.
If you add a new route you need to go to adding routing and add a new server method to handle the new route.

***


**Creating the routes file**

Open the `routes.js` file under your  `api > routes` folder.

Change the file to look like this:
```
var controller = require('../controllers/app_controller');
module.exports = function (webserver) {
    webserver.route('/api/v1/items')
        .post(controller.create_an__item)
        .get(controller.read_all__items);
    webserver.route('/api/v1/items/:id')
        .get(controller.read_an__item)
        .put(controller.update_an__item)
        .delete(controller.delete_an__item);
};
```

We have created a route now pointing to different functions in the controllers file.
The route `HTTP://localhost:3100/api/v1/items` will call the `controller.read_all__items()` function
The route `HTTP://localhost:3100/api/v1/items/1` will call the `controller.read_an__item()` function

Only GET methods can be called directly on the web.
You need something like postman to test your API end points , with POST,PUT and DELETE verbs

***



**Creating the controller file**

Lets update the controller file now.<br>
Open your app_controller.js file and change to the following
```
var _httpresultwrapper = require('../infrastructure/httphandler');
var bll = require('../bll/app_bll');
exports.create_an__item= function (req, res) {
    bll.create_an__item(req, function (jsonResult, haserror, code) {
        _httpresultwrapper(res, jsonResult, haserror, code);
    });
};
exports.read_all__items= function (req, res) {
    bll.read_all__items(req, function (jsonResult, haserror, code) {
        _httpresultwrapper(res, jsonResult, haserror, code);
    });
};
exports.read_an__item= function (req, res) {
    bll.read_an__item(req, function (jsonResult, haserror, code) {
        _httpresultwrapper(res, jsonResult, haserror, code);
    });
};
exports.update_an__item= function (req, res) {
    bll.update_an__item(req, function (jsonResult, haserror, code) {
        _httpresultwrapper(res, jsonResult, haserror, code);
    });
};
exports.delete_an__item= function (req, res) {
    bll.delete_an__item(req, function (jsonResult, haserror, code) {
        _httpresultwrapper(res, jsonResult, haserror, code);
    });
};
```


The controller file is the file the routes method will call , and then we pass this on to the business logic layer to get the values we require.<br>
This abstraction leaves the files in clear functional areas.

***


**Creating the BLL file**

The BLL files will be the files that does all the business logic.<br>
Open your app_bll.js file and add the following code:
```
exports.create_an__item = function (req, callback) {
    callback("create_an__item", false, 200)
};exports.read_all__items = function (req, callback) {
    callback("read all item", false, 200)
};exports.read_an__item = function (req, callback) {
    callback("read_an__item", false, 200)
};exports.update_an__item = function (req, callback) {
    callback("update_an__item", false, 200)
};exports.delete_an__item = function (req, callback) {
    callback("delete_an__item", false, 200)
};
```

The file will be called with the req as a param and then call back with the data , a success bool , and a HTTP status code.



***






Save the files and run the application.

[How to run...](HTTP://github.com/Roche-Olivier/Examples/wiki/Running-a-node-project)

Open your browser and navigate to the following page: HTTP://localhost:3100/api/v1/items/1

You will see the following response on your screen as json<br>
`{"success":true,"httpStatusCode":200,"data":"read_an__item"}`


![Examples and lessons](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite/blob/master/_content/_images/footer.png "Examples and lessons")



