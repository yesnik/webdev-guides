# REST API

## Best practices

### Use nouns, not verbs

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

### Use plural nouns for collections

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

### Use nesting to show relation between resources

We want to show books of author with id = 1:

- `/authors/1/books` - using nesting
- `/books?author_id=1` - using filters 

Too many nested levels may not look too elegant.
