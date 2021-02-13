# Return Types

You can ask the `query()` method to return an array `default`, a Java `ViewReponse` object, or a Java iterator. By default we use the native CFML array type which uses transformations, automatic deserializations and inflations.

```javascript
// Get an iterator of Java objects
iterator = couchbase.query(
    designDocumentName = 'beer', 
    viewName = 'brewery_beers', 
    returnType = 'Iterator' 
);

while( iterator.hasNex() ) {
    writeOutput( iterator.getNext().getKey() );
}
```

