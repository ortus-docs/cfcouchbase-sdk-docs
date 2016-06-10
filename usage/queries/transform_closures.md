# Transform Closures

The `transform` closure will be called in each iteration as we build the native CF array of results. This can be used to normalize data, add or remove data, you name it.

```js
results = couchbase.query(
	designDocumentName = 'beer', 
	viewName = 'brewery_beers', 
	deserialize = false,
	transform = function( row ){
		row.document = deserializeJSON( row.document );
	}
);
```