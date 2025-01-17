# REST API

## Best practices

### 1. Resource naming

**1.1. Endpoints as nouns, not verbs**

URI should refer to a resource that is a thing (noun) instead of referring to an action (verb) because nouns have properties which verbs do not have.

| Good                                                                   | Bad                                                                                    |
|------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| `POST /posts`<br>`GET /posts/1`<br>`PUT /posts/1`<br>`DELETE /posts/1` | `POST /createPost`<br>`GET /getPost/1`<br>`POST /updatePost/1`<br>`POST /deletePost/1` |

But in some cases, we can use verbs. Controller resources are like executable functions with parameters, return values, inputs, and outputs:

```
/cart-management/users/123/cart/checkout
/song-management/users/123/playlist/play
```

**Important:** Ensure that your `GET`, `PUT`, and `DELETE` operations are all **idempotent**. 
There should be no adverse side affects from these operations.

**1.2. Use plural nouns for collections**

| Good                                | Bad                               |
|-------------------------------------|-----------------------------------|
| `GET /cards/1`<br>`GET /products/1` | `GET /card/1`<br>`GET /product/1` |

**1.3. Use hyphens (kebab-case)**

To improve the readability of long URIs use hyphens to split words. Compare:

- `/servers-management/managed-entities/123/install-script-location`
- `/servers_management/managed_entities/123/install_script_location`
- `/serversManagement/managedEntities/123/installScriptLocation`

**1.4. Use nesting to show relation between resources**

- `/authors/1/books`
- `/posts/1/comments`
- `/products/1/reviews`

Too many nested levels may not look too elegant.

### 2. Use JSON in requests and responses

API should accept request payload and send response in JSON format. JSON is a convenient way to transfer data. 
Almost every technology can use it: frontend and backend technologies have libraries that can encode and decode JSON.

### 3. Use pagination

It's not a good idea to send large amount of data through HTTP, because serializing the large JSON objects are expensive. 
That's why it's better to paginate the results.

- `/v1/books?page=1&size=2`

Return additional info:

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

It's good to have a tool that can generate your API's documentation from the source code:

- PHP: [API Platform](https://api-platform.com/)

### 5. Allow filter, sort

Sometimes we need a collection of resource to be sorted or filtered. 
To do this enable sorting, filtering capabilities in resource collection API and pass the input parameters as **query parameters**.

The difference between *URL parameters* and *query parameters*:

- URL parameters are embedded within the URL path to identify specific resources: `/posts/123`, where `123` is an URL param.
- Query parameters are appended after the URL path, starting with a `?`, and are used to pass and filter data within the request.

Use `camelCase` for query params.

#### Filter

```
/products
/products?region=USA
/products?region=USA&brand=ABC

# It's URL encoded version of `/catalog/phone/225?filter[brand][0]=218237&filter[brand][1]=218350`:
/catalog/phone/225?filter%5Bbrand%5D%5B0%5D=218237&filter%5Bbrand%5D%5B1%5D=218350
```

#### Sort

```
/managed-devices?region=USA&sort=updatedAt
/catalog/phone/225/zte?orderBy=-popularity
/catalog/phone/225/zte?orderBy=price
```

### 6. Add support of adding new versions of API

We need an ability to add new versions of API. Otherwise our changes may break clients. 

```
/api/v1/products
/api/v2/products
```

The `v1` endpoint can stay active for people who don't want to change. 
The `v2` endpoint with new features can serve those who are ready to upgrade. This is especially important if our API is public. 

### 7. Send useful response

#### Meaningful errors

You should correctly handle following situations:

- Client hits invalid URL of your API
- Client provides invalid params for your API method
- Client tries to pass additional params that may break your API

#### Correct HTTP response code

See [HTTP response status codes at mozilla.org](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

- `200` - success
- `201` - created. Returned on successful creation of a new resource.
- `202` - accepted. Indicates that the request has been accepted for processing, but the processing has not been completed or not have started yet. 
- `204` - no content. Indicates success but nothing is in the response body, often used for DELETE and PUT operations.
- `400` - bad request. Data issues such as invalid JSON, Domain validation errors, missing data, etc.
- `401` - unauthorized. Error code response for missing or invalid authentication token.
- `403` - forbidden. Error code for when the user is not authorized to perform the operation or the resource is unavailable for some reason (e.g. time constraints, etc.).
- `404` - not found. Used when the requested resource is not found, whether it doesn't exist or if there was a 401 or 403 that, for security reasons, the service wants to mask.
- `405` - method not allowed. Used to indicate that the requested URL exists, but the requested HTTP method is not applicable. For example, POST /users/12345 where the API doesn't support creation of resources this way (with a provided ID). The Allow HTTP header must be set when returning a 405 to indicate the HTTP methods that are supported. In the previous case, the header would look like `Allow: GET, PUT, DELETE`.
- `409` - conflict. Whenever a resource conflict would be caused by fulfilling the request. Duplicate entries, such as trying to create two customers with the same information, and deleting root objects when cascade-delete is not supported.
- `500` - internal server error. Never return this intentionally. The general catch-all error when the server-side throws an exception.
- `503` - service unavailable

More info at [iana.org](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml)

#### Unified response structure

Request `GET /users/1`.

Response (success):

```json
{
  "data": [
    {"id": 1, "login": "kenny"}
  ]
}
```

Response (error):

```json
{
  "error": "No users in the database"
}
```

#### Useful response body

For example, if you make `POST /users/` it'll be useful if server returns: 

```json
{
  "data": {
    "userId": 99
  }
}
```

### 8. Log requests and responses

Log all requests to your API and responses. It will help you to debug.

### 9. Monitor the performance

- Log request execution time.
- Set time limits, and notify developers when API method works too slowly.

### 10. Use headers

- `Content-Type: application/json`
- `Request-Id: x312dxd` - unique identifier value of every HTTP request involved in operation processing, and is generated on the client side and passed to server.
- `X-Correlation-Id: abc1d23` - unique identifier value that is attached to requests and messages that allow reference to a particular transaction or event chain.

#### Authentication

- **API Key**: `X-API-Key: MY_KEY`
- **JWT**: `Authorization: Bearer <token>`

## Useful links

- https://restcookbook.com/
- https://restfulapi.net/
- [Documenting APIs: A guide for technical writers and engineers](https://idratherbewriting.com/learnapidoc/)
