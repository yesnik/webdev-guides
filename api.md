# REST API

## Best practices

### 1. Endpoints as nouns, not verbs

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

### 2. Use plural nouns for collections

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

### 3. Use nesting to show relation between resources

We want to show books of author with id = 1:

- `/authors/1/books` - using nesting
- `/books?author_id=1` - using filters 

Too many nested levels may not look too elegant.

### 4. Use pagination

It's not a good idea to send large amount of data through HTTP, because serializing the large JSON objects are expensive. 
That's why it's better to paginate the results.

### 5. Create documentation for your API

Create useful documentation with examples. 
Describe API endpoints, and describe all operations allowed on each endpoint. 
You can use a tool to do this in an automated way.
