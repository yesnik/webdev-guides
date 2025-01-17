# REST API

## Best practices

### 1. Resource naming

**1.1. Endpoints as nouns, not verbs**

URI should refer to a resource that is a thing (noun) instead of referring to an action (verb) because nouns have properties which verbs do not have.

| Good                                                                   | Bad                                                                                    |
|------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| `POST /posts`<br>`GET /posts/1`<br>`PUT /posts/1`<br>`DELETE /posts/1` | `POST /createPost`<br>`GET /getPost/1`<br>`POST /updatePost/1`<br>`POST /deletePost/1` |

But in some cases, we can use verbs. Controller resources are like executable functions with parameters, return values, inputs, and outputs:

- `/cart-management/users/123/cart/checkout`
- `/song-management/users/123/playlist/play`
- `/v1/auth/login`

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

- `200` - OK. The request succeeded.
- `201` - Created. The request succeeded, and a new resource was created as a result. This is typically the response sent after POST requests, or some PUT requests.
- `202` - Accepted. Indicates that the request has been accepted for processing, but the processing has not been completed or not have started yet. It is intended for cases where another process or server handles the request, or for batch processing.
- `204` - No content. Indicates success but nothing is in the response body, often used for DELETE and PUT operations. The user agent may update its cached headers for this resource with the new ones.
- `301` - Moved Permanently. The URL of the requested resource has been changed permanently. The new URL is given in the response.
- `302` - Found. This response code means that the URI of requested resource has been changed temporarily.
- `304` - Not Modified. This is used for caching purposes. It tells the client that the response has not been modified, so the client can continue to use the same cached version of the response.
- `307` - Temporary Redirect. This has the same semantics as the `302 Found` response code, with the exception that the user agent must not change the HTTP method used: if a POST was used in the first request, a POST must be used in the redirected request.
- `308` - Permanent Redirect. This has the same semantics as the `301 Moved Permanently` HTTP response code, with the exception that the user agent must not change the HTTP method used: if a POST was used in the first request, a POST must be used in the second request.
- `400` - Bad request. Data issues such as invalid JSON, Domain validation errors, missing data, etc.
- `401` - Unauthorized. Error code response for missing or invalid authentication token. The client must authenticate itself to get the requested response.
- `403` - Forbidden. Error code for when the user is not authorized to perform the operation or the resource is unavailable for some reason (e.g. time constraints, etc.). Unlike `401 Unauthorized`, the client's identity is known to the server.
- `404` - Not Found. The server cannot find the requested resource. In the browser, this means the URL is not recognized. In an API, this can also mean that the endpoint is valid but the resource itself does not exist. Servers may also send this response instead of 403 Forbidden to hide the existence of a resource from an unauthorized client.
- `405` - Method Not Allowed. The request method is known by the server but is not supported by the target resource. For example, an API may not allow DELETE on a resource, or the TRACE method entirely. It is used to indicate that the requested URL exists, but the requested HTTP method is not applicable. For example, POST /users/12345 where the API doesn't support creation of resources this way (with a provided ID). The Allow HTTP header must be set when returning a 405 to indicate the HTTP methods that are supported. In the previous case, the header would look like `Allow: GET, PUT, DELETE`.
- `409` - Conflict. Whenever a resource conflict would be caused by fulfilling the request. Duplicate entries, such as trying to create two customers with the same information, and deleting root objects when cascade-delete is not supported.
- `500` - Internal server error. Never return this intentionally. The general catch-all error when the server-side throws an exception.
- `501` - Not Implemented. The request method is not supported by the server and cannot be handled. The only methods that servers are required to support (and therefore that must not return this code) are GET and HEAD.
- `502` - Bad Gateway. This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.
- `503` - Service unavailable. The server is not ready to handle the request. Common causes are a server that is down for maintenance or that is overloaded. Note that together with this response, a user-friendly page explaining the problem should be sent.
- `504` - Gateway Timeout. This error response is given when the server is acting as a gateway and cannot get a response in time.
- `505` - HTTP Version Not Supported. The HTTP version used in the request is not supported by the server.

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
