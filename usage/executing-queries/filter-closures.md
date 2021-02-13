# Filter Closures

You can specify a closure as the `filter` argument that returns true for records that should be included in the final output. This is a great way to filter out any unecessary documents you do not want in the end result array.

```javascript
// Only return breweries
results = client.query(
    designDocumentName = 'beer', 
    viewName = 'brewery_beers', 
    filter = function( row ){
        if( row.document.type == 'brewery' ) {
            return true;
        }
        return false;
    }
);
```

