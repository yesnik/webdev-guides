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

### 4. Create documentation for your API

Create useful documentation with examples. 
Describe API endpoints, and describe all operations allowed on each endpoint. 
You can use a tool to do this in an automated way.

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

### 7. Return useful errors

You should correctly handle following situations:

- Client hits invalid URL of your API
- Client provides invalid params for your API method
- Client tries to pass additional params that may break your API

### 8. Return correct HTTP response code

- `200` - success
- `201` - created
- `400` - bad request
- `401` - unauthorized
- `403` - forbidden
- `404` - not found
- `405` - method not allowed
- `500` - internal server error
- `503` - service unavailable

