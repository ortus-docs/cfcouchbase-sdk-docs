# View Queries

The minimum information you need to execute a view query is the name of the **design document** and the **view** you wish you use.

```javascript
results = client.query( designDocumentName='beer', viewName='brewery_beers' );

for( var result in results ) {
    writeOutput( result.document.name );
    writeOutput( '<br>' );
}
```

Here are the arguments you can pass into the `query()` or `viewQuery()` methods. For the latest and more in-depth information please visit our [API Docs](http://apidocs.ortussolutions.com/cfcouchbase/2.0.0).

| Argument | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| designDocumentName | `string` |  | The name of the design document in your Couchbase server |
| viewName | `string` |  | The name of the view to query from |
| options | `any` | {} | The query options to use for this query. This can be a structure of name-value pairs or an actual Couchbase query options object usually using the `newViewQuery()` method. |
| deserialize | `boolean` | true | If true, it will deserialize the documents if they are valid JSON, else they are ignored. |
| deserializeOptions | `struct` |  | A struct of options to help control how the data is deserialized when populating an object. See ObjectPopulator.cfc for more info. |
| inflateTo | `any` |  | A path to a CFC or closure that produces an object to try to inflate the document results on NON-Reduced views only! |
| filter | `function` |  | A closure or UDF that must return boolean to use to filter out results from the returning array of records, the closure receives a struct that has id, document, key, and value: function\( row \). A true will add the row to the final results. |
| transform | `function` |  | A closure or UDF to use to transform records from the returning array of records, the closure receives a struct that has id, document, key, and value: function\( row \). Since the struct is by reference, you do not need to return anything. |
| returnType | `any` | array | The type of return for us to return to you. Available options: array, native, iterator |

## Results

CFCouchbase will natively transform the data into an array of structs with a specific format. These keys are included in the struct that represents a row. This is the same struct that is returned in the result array and passed into the transform and filter closures.

* `id` - The unique document id. Only avaialble on non-reduced queries
* `document` - The JSON document reinflated back to its original form. Only available on non-reduced views
* `key` - For non-reduced queries, the key emitted from the map function. For reduced views, null.
* `value` - For non-reduced queries, the value emitted from the map function. For reduced views, the output of the reduce function.

## Example

```javascript
// Return 10 records, skipping the first 20.  Force fresh data
results = client.query( 
    designDocumentName='beer', 
    viewName='brewery_beers', 
    options={ limit = 10, offset = 20, stale = 'FALSE' } 
);

// Only return 20 records and skip the reduce function in the view
results = client.query( 
    designDocumentName='beer', 
    viewName='by_location', 
    options={ limit = 20, reduce = false } 
);

// Group results (Will return a single record with the count as the value)
results = client.query( 
    designDocumentName='beer', 
    viewName='brewery_beers', 
    options={ group = true } 
);

// Start at the specified key and sort descending 
results = client.query( 
    designDocumentName='beer', 
    viewName='brewery_beers', 
    options={ sortOrder = 'DESC', startKey = ["aldaris","aldaris-zelta"] } 
);
```

