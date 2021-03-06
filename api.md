# REST API

## Best practices

### 1. Resource naming

**1.1. Endpoints as nouns, not verbs**

URI should refer to a resource that is a thing (noun) instead of referring to an action (verb) because nouns have properties which verbs do not have – similar to resources have attributes.

Good

```
POST /posts
GET /posts/1
PUT /posts/1
DELETE /posts/1
```

Bad

```
POST /createPost
GET /getPost/1
POST /updatePost/1
POST /deletePost/1
```

But in some cases we can use verbs. Controller resources are like executable functions, with parameters and return values, inputs and outputs:

```
/cart-management/users/123/cart/checkout
/song-management/users/123/playlist/play
```
Ensure that your GET, PUT, and DELETE operations are all idempotent. There should be no adverse side affects from these operations.

**1.2. Use plural nouns for collections**

Good

```
GET /cards/1
GET /products/1
```

Bad

```
GET /card/1
GET /product/1
```

**1.3. Use hyphens**

To improve the readability of long URIs use hyphens to split words:

```
# More readable
/inventory-management/managed-entities/123/install-script-location

# Less readable
/inventory-management/managedEntities/123/installScriptLocation
```

**1.4. Use nesting to show relation between resources**

We want to show books of author with id = 1:

- `/authors/1/books` - using nesting
- `/books?author_id=1` - using filters 

Too many nested levels may not look too elegant.

### 2. Use JSON in requests and responses

API should accept request payload and send response in JSON format. JSON is a convenient way to transfer data. 
Almost every technology can use it: frontend and backend technologies have libraries that can encode and decode JSON.

### 3. Use pagination

It's not a good idea to send large amount of data through HTTP, because serializing the large JSON objects are expensive. 
That's why it's better to paginate the results.

**3.1. Return additional info**

```json
{
  "totalItems": 50,
  "page": 1,
  "data": [
    {"id": 1, "title": "Book"},
    {"id": 2, "title": "Pen"}
  ]
}
```

### 4. Create documentation for your API

Create useful documentation with examples. 
Describe API endpoints, and describe all operations allowed on each endpoint. 
You can use a tool to do this in an automated way.

There are different schema formats for API:

- [OpenAPI](https://www.openapis.org/)
- [GraphQL](https://graphql.org/)
- [RAML](https://raml.org/) - RESTful API Modeling Language (RAML) 

There are different tools for creating API documentation:

- [Postman](https://www.postman.com/). It's possible to create API requests there and publish documentation.
- [Swagger Editor](https://editor.swagger.io/)

### 5. Allow filtering, sorting

Sometimes we need a collection of resource to be sorted or filtered. 
To do this enable sorting, filtering capabilities in resource collection API and pass the input parameters as query parameters:

```
/products
/products?region=USA
/products?region=USA&brand=ABC
/managed-devices?region=USA&sort=udated_at
```

### 6. Use versions for API

We should have different versions of API if we're making any changes to them that may break clients. 

```
/api/v1/products
/api/v2/products
```

The `v1` endpoint can stay active for people who don't want to change. The `v2` endpoint with new features can serve those who are ready to upgrade. This is especially important if our API is public. 

### 7. Send useful response

**Meaningful errors**

You should correctly handle following situations:

- Client hits invalid URL of your API
- Client provides invalid params for your API method
- Client tries to pass additional params that may break your API

**Correct HTTP response code**

- `200` - success
- `201` - created. Returned on successful creation of a new resource.
- `204` - no content. Indicates success but nothing is in the response body, often used for DELETE and PUT operations.
- `400` - bad request. Data issues such as invalid JSON, Domain validation errors, missing data, etc.
- `401` - unauthorized. Error code response for missing or invalid authentication token.
- `403` - forbidden. Error code for when the user is not authorized to perform the operation or the resource is unavailable for some reason (e.g. time constraints, etc.).
- `404` - not found. Used when the requested resource is not found, whether it doesn't exist or if there was a 401 or 403 that, for security reasons, the service wants to mask.
- `405` - method not allowed. Used to indicate that the requested URL exists, but the requested HTTP method is not applicable. For example, POST /users/12345 where the API doesn't support creation of resources this way (with a provided ID). The Allow HTTP header must be set when returning a 405 to indicate the HTTP methods that are supported. In the previous case, the header would look like `Allow: GET, PUT, DELETE`.
- `409` - conflict. Whenever a resource conflict would be caused by fulfilling the request. Duplicate entries, such as trying to create two customers with the same information, and deleting root objects when cascade-delete is not supported.
- `500` - internal server error. Never return this intentionally. The general catch-all error when the server-side throws an exception.
- `503` - service unavailable

**Unified response structure**

Request `GET /users`.

Response (success):

```json
{
  "success": true,
  "data": [
    {"id": 1, "login": "kenny"},
    {"id": 1, "login": "leo"},
  ]
}
```

Response (error):

```json
{
  "success": false,
  "error": "No users in the database"
}
```


**Useful response body**

For example, if you make `POST /users/` it'll be useful if server returns: 

```json
{
  "success": true,
  "data": {
    "userId": 99
  }
}
```

### 8. Log requests and responses

It's useful to add an opportunity to log all requests to your API and responses.
It will help you to say what went wrong when some your's API user got an error.

### 9. Monitor the performance

- It's important to set time limits, and notify developers when API method works to slowly.
- Also it's a good idea to log execution time of every request.

