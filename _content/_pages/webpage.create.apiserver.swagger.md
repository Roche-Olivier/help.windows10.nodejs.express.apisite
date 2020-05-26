### [![Index](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/home.png "Index") Index](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite)

## Swagger for your API

You will note that in the navbar we have added a new path `/swagger`<br>
Lets see how we are going to add a swagger document to the API site

**Adding swagger to your API**

> https://swagger.io/tools/swagger-ui/

Firstly we need to add swagger to our application so we can use the package.

In your application root folder type the following:

`> npm install swagger-ui-express`

Open the `swagger.json` file under the swagger folder and add the following code to the file.<br>
```
{
	"swagger": "2.0",
	"info": {
		"description": "",
		"version": "",
		"title": ""
	},
	"host": "",
	"basePath": "",
	"tags": [
		{
			"name": "Health",
			"description": "Ping servers etc , check basic system health."
		},{
			"name": "API heading",
			"description": "The API's will fall under this list."
		}
	],
	"schemes": [
		"http","https"
	],
	"paths": {
		"/items": {
			"post": {
				"tags": [
					"API heading"
				],
				"description": "Create a new item.",
				"parameters": [
					{
						"in": "body",
						"name": "body",
						"description": "Create a new item",
						"required": true,
						"schema": {
							"$ref": "#/definitions/item_model"
						}
					}
				],
				"responses": {
					"201": {
						"description": "Created"
					},
					"400": {
						"description": "Bad Request"
					},
					"503": {
						"description": "Service unavailable"
					}
				}
			},
			"get": {
				"tags": [
					"API heading"
				],
				"description": "Get all the items.",
				"parameters": [],
				"responses": {
					"200": {
						"description": "OK"
					},
					"400": {
						"description": "Bad Request"
					},
					"404": {
						"description": "No records"
					},
					"503": {
						"description": "Service unavailable"
					}
				}
			}
		},
		"/items/{id}": {
			"get": {
				"tags": [
					"API heading"
				],
				"description": "Get a specific item by it's id.",
				"parameters": [
					{
						"name": "id",
						"in": "path",
						"description": "Id to be fetched",
						"required": true,
						"type": "integer",
                        "format": "int32",
                        "example": 1
					}
				],
				"responses": {
					"200": {
						"description": "OK"
					},
					"400": {
						"description": "Bad Request"
					},
					"404": {
						"description": "No records"
					},
					"503": {
						"description": "Service unavailable"
					}
				}
			},
			"put": {
				"tags": [
					"API heading"
				],
				"description": "Update an item with the id supplied.",
				"parameters": [
					{
						"name": "id",
						"in": "path",
						"description": "Id of the item that needs to be edited",
						"required": true,
						"type": "integer",
						"format": "int32"
					},
					{
						"in": "body",
						"name": "body",
						"description": "Model for the update",
						"required": true,
						"schema": {
							"$ref": "#/definitions/item_model"
						}
					}
				],
				"responses": {
					"200": {
						"description": "OK"
					},
					"400": {
						"description": "Bad Request"
					},
					"503": {
						"description": "Service unavailable"
					}
				}
			},
			"delete": {
				"tags": [
					"API heading"
				],
				"description": "Delete an item.",
				"parameters": [
					{
						"name": "id",
						"in": "path",
						"description": "Id of the item that needs to be removed",
						"required": true,
						"type": "integer",
						"format": "int32",
						"example": 1
					}
				],
				"responses": {
					"204": {
						"description": "No content"
					},
					"400": {
						"description": "Bad Request"
					},
					"503": {
						"description": "Service unavailable"
					}
				}
			}
		}
	},
	"definitions": {
		"item_model": {
			"type": "object",
			"required": [
				"name",
				"description"
			],
			"properties": {
				"name": {
					"type": "string",
					"example": "jack"
				},
				"description": {
					"type": "string",
					"example": "jack description"
				}
			}
		}
    }
}
```

Now that you have added the swagger.json we need to add the swagger path to the server

Open the `index.js` file under the application root folder change the file to look like this.<br>
```
const express = require('./node_modules/express')
const path = require('path')
const app = express()

// SWAGGER
//-----------------------------------------------------------------------
process.env.SWAGGER_FILE='swagger.json'
process.env.SWAGGER_HOST='localhost:3100'
var pjson = require('./package.json');
const swaggerUi = require('swagger-ui-express');
const swaggerDocument = require('./swagger/' + process.env.SWAGGER_FILE);
var showExplorer = false;
var options = {
  validatorUrl: null
};
swaggerDocument.host = process.env.SWAGGER_HOST
swaggerDocument.info.version = pjson.version
swaggerDocument.info.description = pjson.name
swaggerDocument.info.title = pjson.description

swaggerDocument.basePath = '/api/v1/'
app.use('/swagger', swaggerUi.serve, swaggerUi.setup(swaggerDocument, showExplorer, options));
//-----------------------------------------------------------------------


app.use('/css/', express.static(path.join(__dirname, '/content/css')));
app.use('/images/', express.static(path.join(__dirname, '/content/images')));
app.use('/js/', express.static(path.join(__dirname, '/content/js')));
app.use('/pages/', express.static(path.join(__dirname, '/content/pages')));

app.get('/', function (req, res) { res.sendFile(path.join(__dirname + '/content/pages/index.html')); });
app.get('/home/', function (req, res) { res.sendFile(path.join(__dirname + '/content/pages/index.html')); });

var api_routes = require('./api/routes/app_route');
api_routes(app);

app.listen(3100)
```
![Examples and lessons](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite/blob/master/_content/_images/footer.png "Examples and lessons")



