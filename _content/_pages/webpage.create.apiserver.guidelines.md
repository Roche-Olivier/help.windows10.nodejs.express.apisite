### [![Index](https://github.com/Roche-Olivier/help.windows10.nodejs.basics/blob/master/_content/_images/home.png "Index") Index](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite)

## Some general API guidelines


RESOURCES:
==========

JSON API Latest Specification (v1.0)
http://jsonapi.org/format/

Best Practices for Designing a Pragmatic RESTful API<br>
http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api

JSON API documents are defined in JavaScript Object Notation (JSON) [RFC7159].
https://tools.ietf.org/html/rfc7159

Here are some extract that i found helpful:

Introduction
============

JSON API is a specification for how a client should request that resources be fetched or modified, and how a server should respond to those requests.

JSON API is designed to minimize both the number of requests and the amount of data transmitted between clients and servers. This efficiency is achieved without compromising readability, flexibility, or discoverability.

JSON API requires use of the JSON API media type (application/vnd.api+json) for exchanging data.

Key requirements for the API
============================

> Many of the API design opinions found on the web are academic discussions revolving around subjective interpretations of fuzzy standards as opposed to what makes sense in the real world.<br>
My goal with this post is to describe best practices for a pragmatic API designed for today's web applications. 
<br><br>
 To help guide the decision making process, I've written down some requirements that the API must strive for:<br>
 -It should use web standards where they make sense<br>
 -It should be friendly to the developer and be explorable via a browser address bar<br>
 -It should be simple, intuitive and consistent to make adoption not only easy but pleasant<br>
 -It should provide enough flexibility to power majority of the UI<br>
 -It should be efficient, while maintaining balance with the other requirements<br>
 -An API is a developer's UI - just like any UI, it's important to ensure the user's experience is thought out carefully!<br>

Mock Responses
--------------

> It is suggested that each resource accept a 'mock' parameter on the testing server. Passing this parameter should return a mock data response (bypassing the backend).<br>
Implementing this feature early in development ensures that the API will exhibit consistent behavior, supporting a test driven development methodology.<br>
Note: If the mock parameter is included in a request to the production environment, an error should be raised.

1.Use nouns but no verbs
========================

**Use RESTful URLs and actions**

> If there's one thing that has gained wide adoption, it's RESTful principles. These were first introduced by Roy Fielding in Chapter 5 of his dissertation on network based software architectures.<br>
> The key principles of REST involve separating your API into logical resources. These resources are manipulated using HTTP requests where the method (GET, POST, PUT, PATCH, DELETE) has specific meaning.<br>
> <br>
> For an easy understanding use this structure for every resource:<br>
> <br>
> But what can I make a resource? Well, these should be nouns (not verbs!) that make sense from the perspective of the API consumer. Although your internal models may map neatly to resources, it isn't necessarily a one-to-one mapping. The key here is to not leak irrelevant implementation details out to your API! <br>
> <br>
> Once you have your resources defined, you need to identify what actions apply to them and how those would map to your API. RESTful principles provide strategies to handle CRUD actions using HTTP methods mapped as follows:<br>
> <br>
> GET /tickets        - Retrieves a list of tickets<br>
> GET /tickets/12     - Retrieves a specific ticket<br>
> POST /tickets       - Creates a new ticket<br>
> PUT /tickets/12     - Updates ticket #12<br>
> PATCH /tickets/12   - Partially updates ticket #12<br>
> DELETE /tickets/12  - Deletes ticket #12<br>
> The great thing about REST is that you're leveraging existing HTTP methods to implement significant functionality on just a single /tickets endpoint. There are no method naming conventions to follow and the URL structure is clean & clear.<br>
> <br>
> **Do not use verbs**<br>
> <br>
> /getAllCars<br>
> /createNewCar<br>
> /deleteAllRedCars<br>
> <br>
> Use PUT, POST and DELETE methods  instead of the GET method to alter the state.<br>
> **Do not use GET for state changes**<br>
> <br>
> GET /users/711?activate or<br>
> GET /users/711/activate<br>
> <br>
> **Updates & creation should return a resource representation**<br>
> <br>
> A PUT, POST or PATCH call may make modifications to fields of the underlying resource that weren't part of the provided parameters (for example: created_at or updated_at timestamps). To prevent an API consumer from having to hit the API again for an updated representation, have the API return the updated (or created) representation as part of the response.<br>
> <br>
> In case of a POST that resulted in a creation, use a HTTP 201 status code and include a Location header that points to the URL of the new resource.<br>
> <br>
> **Overriding the HTTP method**<br>
> <br>
> Some HTTP clients can only work with simple GET and POST requests. To increase accessibility to these limited clients, the API needs a way to override the HTTP method. Although there aren't any hard standards here, the popular convention is to accept a request header X-HTTP-Method-Override with a string value containing one of PUT, PATCH or DELETE.<br>
> <br>
> Note that the override header should only be accepted on POST requests. GET requests should never change data on the server!<br>

2.Endpoint name be singular or plural?
======================================

> The keep-it-simple rule applies here. Although your inner-grammatician will tell you it's wrong to describe a single instance of a resource using a plural, the pragmatic answer is to keep the URL format consistent and always use a plural. Not having to deal with odd pluralization (person/people, goose/geese) makes the life of the API consumer better and is easier for the API provider to implement (as most modern frameworks will natively handle /tickets and /tickets/12 under a common controller).<br>
> Do not mix up singular and plural nouns. Keep it simple and use only plural nouns for all resources.<br>
> <br>
> /cars instead of /car<br>
> /users instead of /user<br>
> /products instead of /product<br>
> /settings instead of /setting<br>

3.Deal with relations? 
======================

> If a relation can only exist within another resource, RESTful principles provide useful guidance. 
> Let's look at this with an example. A ticket consists of a number of messages. These messages can be logically mapped to the /tickets endpoint as follows:<br>
> <br>
> GET /tickets/12/messages - Retrieves list of messages for ticket #12<br>
> GET /tickets/12/messages/5 - Retrieves message #5 for ticket #12<br>
> POST /tickets/12/messages - Creates a new message in ticket #12<br>
> PUT /tickets/12/messages/5 - Updates message #5 for ticket #12<br>
> PATCH /tickets/12/messages/5 - Partially updates message #5 for ticket #12<br>
> DELETE /tickets/12/messages/5 - Deletes message #5 for ticket #12<br>
> <br>
> Alternatively, if a relation can exist independently of the resource, it makes sense to just include an identifier for it within the output representation of the resource. <br>
> The API consumer would then have to hit the relation's endpoint. However, if the relation is commonly requested alongside the resource, the API could offer functionality to automatically embed the relation's representation and avoid the second hit to the API.<br>
> If a resource is related to another resource use subresources.<br>
> <br>
> GET /cars/711/drivers/ Returns a list of drivers for car 711<br>
> GET /cars/711/drivers/4 Returns driver #4 for car 711<br>


4.Actions that don't fit into CRUD operations?
==============================================

> This is where things can get fuzzy. There are a number of approaches:<br>
> <br>
> Restructure the action to appear like a field of a resource. This works if the action doesn't take parameters. For example an activate action could be mapped to a boolean activated field and updated via a PATCH to the resource.<br>
> Treat it like a sub-resource with RESTful principles.<br> For example, GitHub's API lets you star a gist with PUT /gists/:id/star and unstar with DELETE /gists/:id/star.<br><br>
> Sometimes you really have no way to map the action to a sensible RESTful structure. For example, a multi-resource search doesn't really make sense to be applied to a specific resource's endpoint.<br> In this case, /search would make the most sense even though it isn't a resource.<br> This is OK - just do what's right from the perspective of the API consumer and make sure it's documented clearly to avoid confusion.<br>


5.Pretty print & ensure gzip is supported
=========================================

> An API that provides white-space compressed output isn't very fun to look at from a browser. Although some sort of query parameter (like ?pretty=true) could be provided to enable pretty printing, an API that pretty prints by default is much more approachable. The cost of the extra data transfer is negligible, especially when you compare to the cost of not implementing gzip.<br>
> <br>
> Consider some use cases: What if an API consumer is debugging and has their code print out data it received from the API - It will be readable by default. Or if the consumer grabbed the URL their code was generating and hit it directly from the browser - it will be readable by default. These are small things. Small things that make an API pleasant to use!
> The output files have the following sizes:<br>
> <br>
> without-whitespace.txt - 1252 bytes<br>
> with-whitespace.txt - 1369 bytes<br>
> without-whitespace.txt.gz - 496 bytes<br>
> with-whitespace.txt.gz - 509 bytes<br>
> <br>
> In this example, the whitespace increased the output size by 8.5% when gzip is not in play and 2.6% when gzip is in play. On the other hand, the act of gzipping in itself provided over 60% in bandwidth savings. Since the cost of pretty printing is relatively small, it's best to pretty print by default and ensure gzip compression is supported!<br>
> <br>
> To further hammer in this point, Twitter found that there was an 80% savings (in some cases) when enabling gzip compression on their Streaming API. Stack Exchange went as far as to never return a response that's not compressed!<br>


6.snake_case vs camelCase for field names
=========================================

> If you're using JSON (JavaScript Object Notation) as your primary representation format, the "right" thing to do is to follow JavaScript naming conventions - and that means camelCase for field names! If you then go the route of building client libraries in various languages, it's best to use idiomatic naming conventions in them - camelCase for C# & Java, snake_case for python & ruby.<br>
> <br>
> Food for thought: I've always felt that snake_case is easier to read than JavaScript's convention of camelCase. I just didn't have any evidence to back up my gut feelings, until now. Based on an eye tracking study on camelCase and snake_case (PDF) from 2010, snake_case is 20% easier to read than camelCase! That impact on readability would affect API explorability and examples in documentation.<br>
> <br>
> Many popular JSON APIs use snake_case. I suspect this is due to serialization libraries following naming conventions of the underlying language they are using. Perhaps we need to have JSON serialization libraries handle naming convention transformations.<br>


7.JSON encoded POST, PUT & PATCH bodies
=======================================

> If you're following the approach in this post, then you've embraced JSON for all API output. Let's consider JSON for API input.<br>
> <br>
> Many APIs use URL encoding in their API request bodies. URL encoding is exactly what it sounds like - request bodies where key value pairs are encoded using the same conventions as one would use to encode data in URL query parameters. This is simple, widely supported and gets the job done.<br>
> <br>
> However, URL encoding has a few issues that make it problematic. It has no concept of data types. This forces the API to parse integers and booleans out of strings. Furthermore, it has no real concept of hierarchical structure. Although there are some conventions that can build some structure out of key value pairs (like appending [ ] to a key to represent an array), this is no comparison to the native hierarchical structure of JSON.<br>
> <br>
> If the API is simple, URL encoding may suffice. However, complex APIs should stick to JSON for their API input. Either way, pick one and be consistent throughout the API.<br>
> <br>
> An API that accepts JSON encoded POST, PUT & PATCH requests should also require the Content-Type header be set to application/json or throw a 415 Unsupported Media Type HTTP status code.<br>


8.Pagination
============

> Envelope loving APIs typically include pagination data in the envelope itself. And I don't blame them - until recently, there weren't many better options. The right way to include pagination details today is using the Link header introduced by RFC 5988.<br>
> <br>
> An API that uses the Link header can return a set of ready-made links so the API consumer doesn't have to construct links themselves. This is especially important when pagination is cursor based. Here is an example of a Link header used properly, grabbed from GitHub's documentation:<br>
> <br>
> <br>
> Link: <https://api.github.com/user/repos?page=3&per_page=100>; rel="next", <https://api.github.com/user/repos?page=50&per_page=100>; rel="last"<br>
> But this isn't a complete solution as many APIs do like to return the additional pagination information, like a count of the total number of available results. An API that requires sending a count can use a custom HTTP header like X-Total-Count.<br>
> <br>
> Use limit and offset. It is flexible for the user and common in leading databases. The default should be limit=20 and offset=0<br>
> <br>
> GET /cars?offset=10&limit=5<br>
> To send the total entries back to the user use the custom HTTP header: X-Total-Count.<br>
> <br>
> Links to the next or previous page should be provided in the HTTP header link as well. It is important to follow this link header values instead of constructing your own URLs.<br>
> <br>
> Link: <https://blog.mwaysolutions.com/sample/api/v1/cars?offset=15&limit=5>; rel="next",<br>
> <https://blog.mwaysolutions.com/sample/api/v1/cars?offset=50&limit=3>; rel="last",<br>
> <https://blog.mwaysolutions.com/sample/api/v1/cars?offset=0&limit=5>; rel="first",<br>
> <https://blog.mwaysolutions.com/sample/api/v1/cars?offset=5&limit=5>; rel="prev",<br>
 

9.Auto loading related resource representations
===============================================

> There are many cases where an API consumer needs to load data related to (or referenced) from the resource being requested. Rather than requiring the consumer to hit the API repeatedly for this information, there would be a significant efficiency gain from allowing related data to be returned and loaded alongside the original resource on demand.<br>
> <br>
> However, as this does go against some RESTful principles, we can minimize our deviation by only doing so based on an embed (or expand) query parameter.<br>
> <br>
> In this case, embed would be a comma separated list of fields to be embedded. Dot-notation could be used to refer to sub-fields. For example:<br>
> <br>
> GET /tickets/12?embed=customer.name,assigned_user<br>
> <br>
> This would return a ticket with additional details embedded, like:<br>
> <br>
> <br>
> {<br>
>   "id" : 12,<br>
>   "subject" : "I have a question!",<br>
>   "summary" : "Hi, ....",<br>
>   "customer" : {<br>
>     "name" : "Bob"<br>
>   },<br>
>   assigned_user: {<br>
>    "id" : 42,<br>
>    "name" : "Jim",<br>
>   }<br>
> }<br>
> Of course, ability to implement something like this really depends on internal complexity. This kind of embedding can easily result in an N+1 select issue.<br>


10.Rate limiting
================

> To prevent abuse, it is standard practice to add some sort of rate limiting to an API. RFC 6585 introduced a HTTP status code 429 Too Many Requests to accommodate this.<br>
> <br>
> However, it can be very useful to notify the consumer of their limits before they actually hit it. This is an area that currently lacks standards but has a number of popular conventions using HTTP response headers.<br>
> <br>
> At a minimum, include the following headers (using Twitter's naming conventions as headers typically don't have mid-word capitalization):<br>
> <br>
> X-Rate-Limit-Limit - The number of allowed requests in the current period<br>
> X-Rate-Limit-Remaining - The number of remaining requests in the current period<br>
> X-Rate-Limit-Reset - The number of seconds left in the current period<br>
> Why is number of seconds left being used instead of a time stamp for X-Rate-Limit-Reset?<br>
> <br>
> A timestamp contains all sorts of useful but unnecessary information like the date and possibly the time-zone. An API consumer really just wants to know when they can send the request again & the number of seconds answers this question with minimal additional processing on their end. It also avoids issues related to clock skew.<br>
> <br>
> Some APIs use a UNIX timestamp (seconds since epoch) for X-Rate-Limit-Reset. Don't do this!<br>
> <br>
> Why is it bad practice to use a UNIX timestamp for X-Rate-Limit-Reset?<br>
> <br>
> The HTTP spec already specifies using RFC 1123 date formats (currently being used in Date, If-Modified-Since & Last-Modified HTTP headers). If we were to specify a new HTTP header that takes a timestamp of some sort, it should follow RFC 1123 conventions instead of using UNIX timestamps.<br>


12.HTTP status codes
====================

> HTTP defines a bunch of meaningful status codes that can be returned from your API. These can be leveraged to help the API consumers route their responses accordingly. I've curated a short list of the ones that you definitely should be using:
> <br>
> 200 OK - Response to a successful GET, PUT, PATCH or DELETE. Can also be used for a POST that doesn't result in a creation.<br>
> 201 Created - Response to a POST that results in a creation. Should be combined with a Location header pointing to the location of the new resource<br>
> 204 No Content - Response to a successful request that won't be returning a body (like a DELETE request)<br>
> <br>
> 304 Not Modified - Used when HTTP caching headers are in play<br>
> <br>
> 400 Bad Request - The request is malformed, such as if the body does not parse<br>
> 401 Unauthorized - When no or invalid authentication details are provided. Also useful to trigger an auth popup if the API is used from a browser<br>
> 403 Forbidden - When authentication succeeded but authenticated user doesn't have access to the resource<br>
> 404 Not Found - When a non-existent resource is requested<br>
> 405 Method Not Allowed - When an HTTP method is being requested that isn't allowed for the authenticated user<br>
> 410 Gone - Indicates that the resource at this end point is no longer available. Useful as a blanket response for old API versions<br>
> 415 Unsupported Media Type - If incorrect content type was provided as part of the request<br>
> 422 Unprocessable Entity - Used for validation errors<br>
> 429 Too Many Requests - When a request is rejected due to rate limiting<br>
> <br>
> 500 – Internal Server Error – API developers should avoid this error. If an error occurs in the global catch blog, the stracktrace should be logged and not returned as response.<br>

13.Authentication
=================

> A RESTful API should be stateless. This means that request authentication should not depend on cookies or sessions. Instead, each request should come with some sort authentication credentials.<br>
> <br>
> By always using SSL, the authentication credentials can be simplified to a randomly generated access token that is delivered in the user name field of HTTP Basic Auth. The great thing about this is that it's completely browser explorable - the browser will just popup a prompt asking for credentials if it receives a 401 Unauthorized status code from the server.<br>
> Today, your web APIs can get accessed from anywhere there is internet (like libraries, coffee shops, airports among others). Not all of these are secure. Many don't encrypt communications at all, allowing for easy eavesdropping or impersonation if authentication credentials are hijacked.<br>
> Another advantage of always using SSL is that guaranteed encrypted communications simplifies authentication efforts - you can get away with simple access tokens instead of having to sign each API request.<br>
> One thing to watch out for is non-SSL access to API URLs. Do not redirect these to their SSL counterparts. Throw a hard error instead! The last thing you want is for poorly configured clients to send requests to an unencrypted endpoint, just to be silently redirected to the actual encrypted endpoint.<br>
> <br>
> However, this token-over-basic-auth method of authentication is only acceptable in cases where it's practical to have the user copy a token from an administration interface to the API consumer environment. In cases where this isn't possible, OAuth 2 should be used to provide secure token transfer to a third party. OAuth 2 uses Bearer tokens & also depends on SSL for its underlying transport encryption.<br>
> <br>
> An API that needs to support JSONP will need a third method of authentication, as JSONP requests cannot send HTTP Basic Auth credentials or Bearer tokens. In this case, a special query parameter access_token can be used. Note: there is an inherent security issue in using a query parameter for the token as most web servers store query parameters in server logs.<br>
> <br>
> For what it's worth, all three methods above are just ways to transport the token across the API boundary. The actual underlying token itself could be identical.<br>


14.Documentation
================

> An API is only as good as its documentation. The docs should be easy to find and publically accessible. Most developers will check out the docs before attempting any integration effort. When the docs are hidden inside a PDF file or require signing in, they're not only difficult to find but also not easy to search.<br>
> <br>
> The docs should show examples of complete request/response cycles. Preferably, the requests should be pastable examples - either links that can be pasted into a browser or curl examples that can be pasted into a terminal. GitHub and Stripe do a great job with this.<br>
> <br>
> Once you release a public API, you've committed to not breaking things without notice. The documentation must include any deprecation schedules and details surrounding externally visible API updates. Updates should be delivered via a blog (i.e. a changelog) or a mailing list (preferably both!).<br>

15.Versioning
=============

> Always version your API. Versioning helps you iterate faster and prevents invalid requests from hitting updated endpoints. It also helps smooth over any major API version transitions as you can continue to offer old API versions for a period of time.<br>
> <br>
> There are mixed opinions around whether an API version should be included in the URL or in a header. Academically speaking, it should probably be in a header. However, the version needs to be in the URL to ensure browser explorability of the resources across versions (remember the API requirements specified at the top of this post?).<br>
> <br>
> I'm a big fan of the approach that Stripe has taken to API versioning - the URL has a major version number (v1), but the API has date based sub-versions which can be chosen using a custom HTTP request header. In this case, the major version provides structural stability of the API as a whole while the sub-versions accounts for smaller changes (field deprecations, endpoint changes, etc).<br>
> <br>
> An API is never going to be completely stable. Change is inevitable. What's important is how that change is managed. Well documented and announced multi-month deprecation schedules can be an acceptable practice for many APIs. It comes down to what is reasonable given the industry and possible consumers of the API.<br>
> <br>
> Make the API Version mandatory and do not release an unversioned API. Use a simple ordinal number and avoid dot notation such as 2.5.<br>
> <br>
> We are using the url for the API versioning starting with the letter „v“<br>
> /blog/api/v1<br>


16.Result filtering, sorting & searching
========================================

> Mobile clients display just a few attributes in a list. They don’t need all attributes of a resource. Give the API consumer the ability to choose returned fields. This will also reduce the network traffic and speed up the usage of the API.<br>
> <br>
> It's best to keep the base resource URLs as lean as possible. Complex result filters, sorting requirements and advanced searching (when restricted to a single type of resource) can all be easily implemented as query parameters on top of the base URL. Let's look at these in more detail:<br>
> <br>
> Filtering: Use a unique query parameter for each field that implements filtering. For example, when requesting a list of tickets from the /tickets endpoint, you may want to limit these to only those in the open state. This could be accomplished with a request like GET /tickets?state=open. Here, state is a query parameter that implements a filter.<br>
> <br>
> Sorting: Similar to filtering, a generic parameter sort can be used to describe sorting rules. Accommodate complex sorting requirements by letting the sort parameter take in list of comma separated fields, each with a possible unary negative to imply descending sort order. Let's look at some examples:<br>
> <br>
> Allow ascending and descending sorting over multiple fields.<br>
> <br>
> GET /cars?sort=-manufactorer,+model<br>
> This returns a list of cars sorted by descending manufacturers and ascending models.<br>
> <br>
> GET /tickets?sort=-priority - Retrieves a list of tickets in descending order of priority<br>
> GET /tickets?sort=-priority,created_at - Retrieves a list of tickets in descending order of priority. Within a specific priority, older tickets are ordered first<br>
> Searching: Sometimes basic filters aren't enough and you need the power of full text search. Perhaps you're already using ElasticSearch or another Lucene based search technology. When full text search is used as a mechanism of retrieving resource instances for a specific type of resource, it can be exposed on the API as a query parameter on the resource's endpoint. Let's say q. Search queries should be passed straight to the search engine and API output should be in the same format as a normal list result.<br>
> <br>
> Combining these together, we can build queries like:<br>
> <br>
> GET /tickets?sort=-updated_at - Retrieve recently updated tickets<br>
> GET /tickets?state=closed&sort=-updated_at - Retrieve recently closed tickets<br>
> GET /tickets?q=return&state=open&sort=-priority,created_at - Retrieve the highest priority open tickets mentioning the word 'return'<br>
> Aliases for common queries<br>
> <br>
> To make the API experience more pleasant for the average consumer, consider packaging up sets of conditions into easily accessible RESTful paths. For example, the recently closed tickets query above could be packaged up as GET /tickets/recently_closed<br>


17.Limiting which fields are returned by the API
================================================

> The API consumer doesn't always need the full representation of a resource. The ability select and chose returned fields goes a long way in letting the API consumer minimize network traffic and speed up their own usage of the API.<br>
> <br>
> Use a fields query parameter that takes a comma separated list of fields to include. For example, the following request would retrieve just enough information to display a sorted listing of open tickets:<br>
> <br>
> GET /tickets?fields=id,subject,customer_name,updated_at&state=open&sort=-updated_at<br>



18.Caching
==========

> HTTP provides a built-in caching framework! All you have to do is include some additional outbound response headers and do a little validation when you receive some inbound request headers.<br>
> <br>
> There are 2 approaches: ETag and Last-Modified<br>
> <br>
> ETag: When generating a request, include a HTTP header ETag containing a hash or checksum of the representation. This value should change whenever the output representation changes. Now, if an inbound HTTP requests contains a If-None-Match header with a matching ETag value, the API should return a 304 Not Modified status code instead of the output representation of the resource.<br>
> <br>
> Last-Modified: This basically works like to ETag, except that it uses timestamps. The response header Last-Modified contains a timestamp in RFC 1123 format which is validated against If-Modified-Since. Note that the HTTP spec has had 3 different acceptable date formats and the server should be prepared to accept any one of them.<br>



19.Errors
=========

> It is hard to work with an API that ignores error handling. Pure returning of a HTTP 500 with a stacktrace is not very helpful.<br>
> <br>
> Just like an HTML error page shows a useful error message to a visitor, an API should provide a useful error message in a known consumable format. The representation of an error should be no different than the representation of any resource, just with its own set of fields.<br>
> <br>
> The API should always return sensible HTTP status codes. API errors typically break down into 2 types: 400 series status codes for client issues & 500 series status codes for server issues. At a minimum, the API should standardize that all 400 series errors come with consumable JSON error representation. If possible (i.e. if load balancers & reverse proxies can create custom error bodies), this should extend to 500 series status codes.<br>
> <br>
> A JSON error body should provide a few things for the developer - a useful error message, a unique error code (that can be looked up for more details in the docs) and possibly a detailed description. JSON output representation for something like this would look like:<br>
> <br>
> <br>
> {<br>
>   "code" : 1234,<br>
>   "message" : "Something bad happened :(",<br>
>   "description" : "More details about the error here"<br>
> }<br>
> Validation errors for PUT, PATCH and POST requests will need a field breakdown. This is best modeled by using a fixed top-level error code for validation failures and providing the detailed errors in an additional errors field, like so:<br>
> <br>
> <br>
> {<br>
>   "code" : 1024,<br>
>   "message" : "Validation Failed",<br>
>   "errors" : [<br>
>     {<br>
>       "code" : 5432,<br>
>       "field" : "first_name",<br>
>       "message" : "First name cannot have fancy characters"<br>
>     },<br>
>     {<br>
>        "code" : 5622,<br>
>        "field" : "password",<br>
>        "message" : "Password cannot be blank"<br>
>     }<br>
>   ]<br>
> }<br>
> <br>
> Use error payloads<br>
> <br>
> All exceptions should be mapped in an error payload. Here is an example how a JSON payload should look like.<br>
> <br>
> 01{<br>
> 02  "errors": [<br>
> 03   {<br>
> 04    "userMessage": "Sorry, the requested resource does not exist",<br>
> 05    "internalMessage": "No car found in the database",<br>
> 06    "code": 34,<br>
> 07    "more info": "http://dev.mwaysolutions.com/blog/api/v1/errors/12345"<br>
> 08   }<br>
> 09  ]<br>
> 10} <br>


20.In Summary
=============
<br>
An API is a user interface for developers. Put the effort in to ensure it's not just functional but pleasant to use.<br>
![Examples and lessons](https://github.com/Roche-Olivier/help.windows10.nodejs.express.apisite/blob/master/_content/_images/footer.png "Examples and lessons")



