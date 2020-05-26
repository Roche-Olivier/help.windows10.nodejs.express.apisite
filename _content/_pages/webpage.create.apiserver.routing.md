### [![Index](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/home.png "Index") Index](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite)

## Server routing


Create a base API site.<br>
[API site : Setup and folder structure](https://github.com/Roche-Olivier/Examples/wiki/API-site-:-Setup-and-folder-structure)


***

We have a folder structure and a nice main page now , lets start adding some API end points to the site.

In your index.js file add the following:
```
var api_routes = require('./api/routes/app_route');
api_routes(app);
```

We are adding the location to a method and passing the server in as a parameter.

***


**Creating the routes file**

Open the `routes.js` file under your  `api > routes` folder.

Add the following code to the routes file:
```
var controller = require('../controllers/app_controller');
module.exports = function (webserver) {
    webserver.route('/api/v1/items')
        .get(controller.read_all__items);
};
```

We have created a route now pointing to a function in the controllers file.
The route `http://localhost:3100/api/v1/items` will call the `controller.read_all__items()` function

***



**Creating the controller file**

Lets create the controller file now.<br>
Open your app_controller.js file and add the following code:
```
var _httpresultwrapper = require('../infrastructure/httphandler');
var bll = require('../bll/app_bll');

exports.read_all__items= function (req, res) {
    bll.read_all__items(req, function (jsonResult, haserror, code) {
        _httpresultwrapper(res, jsonResult, haserror, code);
    });
};
```

Add the following code in the `httphandler.js` file
```
module.exports = function (res,jsonResult, haserror, code) {
    if (haserror) { 
        res.status(code).type('application/json').json({success: false, httpStatusCode: code, error: { message: jsonResult }});
    } else {
        res.status(code).type('application/json').json({success: true, httpStatusCode: code, data: jsonResult });
    }
}
```

This will wrap the results in a specific way , so we can change things in one place if it needs to change.

The controller fil is the file the routes method will call , and then we pass this on to the business logic layer to get the values we require.<br>
This abstraction leaves the files in clear functional areas.

***


**Creating the bll file**

The BLL files will be the files that does all the busoness logic.<br>
Open your app_bll.js file and add the following code:
```
exports.read_all__items = function (req, callback) {
    callback("read all item", false, 200)
};
```

The file will be called with the req as a param and then call back with the data , a success bool , and a HTTP status code.



***






Save the files and run the application.

[How to run...](HTTP://github.com/Roche-Olivier/Examples/wiki/Running-a-node-project)

Open your browser and navigate to the following page: HTTP://localhost:3100/api/v1/items

You will see the following response on your screen as json<br>
`{"success":true,"httpStatusCode":200,"data":"read all item"}`


![Examples and lessons](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite/blob/master/_content/_images/footer.png "Examples and lessons")



