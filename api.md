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

### 2. Use pagination

It's not a good idea to send large amount of data through HTTP, because serializing the large JSON objects are expensive. 
That's why it's better to paginate the results.

### 3. Create documentation for your API

Create useful documentation with examples. 
Describe API endpoints, and describe all operations allowed on each endpoint. 
You can use a tool to do this in an automated way.

### 4. Use query params to filter, sort URI collection

Sometimes we need a collection of resource to be sorted, filtered or limited 
based on some certain resource attribute. 
To do this enable sorting, filtering and pagination capabilities in resource collection API and pass the input parameters as query parameters:

```
/products
/products?region=USA
/products?region=USA&brand=ABC
/managed-devices?region=USA&sort=udated_at
```
