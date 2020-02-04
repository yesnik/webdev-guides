# JSON LD

JSON-LD (JavaScript Object Notation for Linked Data) is a "standard" that describes a few extra fields that your JSON can have.
They starts with `@`, that help machines to learn more about your API.

```json
{ 
  "@context":"\/api\/contexts\/Product",
  "@id":"\/api\/product\/12",
  "@type":"Product",
  "id":2,
  "title":"Book about PHP",
  "description":"Learn more about PHP",
  "price":300,
  "createdAt":"2020-02-03T12:58:14+05:00",
  "isPublished":true
}
```

## Fields

### @id

Every resource should have an `@id` field. Any HTTP client, that understands JSON-LD will know to look for `@id`. 

It's the official "key" for the unique identifier.
This URL is actually called an **IRI**: Internationalized Resource Identifier.

### @type

When we create a class in PHP instead of just an array, we are giving our data a `type`. 
It allows us to know exactly what type of data we're dealing with and we can look at the class 
to learn more about its properties. 

The `@type` field transforms this data from a structureless array into a concrete class 
that we can understand.

### @context

This field contains URL for details about the fields used in the data.
