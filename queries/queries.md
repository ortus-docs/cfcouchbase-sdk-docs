#Executing Queries

One of the most powerful parts of Couchbase Server is the ability to define [http://docs.couchbase.com/couchbase-manual-2.0/#views-and-indexes views (or indexes)] on your data and execute queries against your buckets of data.  We do not cover all the basics and intricacies of views and indexes, for that please visit the official [http://docs.couchbase.com/couchbase-manual-2.0/#views-and-indexes Couchbase Docs].  We focus on how to leverage them from CFCouchbase.  The minimum information you need to execute a query is the name of the **design document** and the **view** you wish you use.

```coldfusion
results = client.query( designDocumentName='beer', viewName='brewery_beers' );

for( var result in results ) {
	writeOutput( result.document.name );
	writeOutput( '<br>' );
}
```

Here are the arguments you can pass into the `query()` method.  For the latest and more in-depth information please visit our [API Docs](http://apidocs.ortussolutions.com/cfcouchbase/2.0.0).

| Argument | Type | Default | Description |
| -- | -- | -- | -- |
| designDocumentName | `string`   |       | The name of the design document in your Couchbase server |
| viewName           | `string`   |       | The name of the view to query from |
| options            | `any`      | {}    | The query options to use for this query. This can be a structure of name-value pairs or an actual Couchbase query options object usually using the `newQuery()` method. |
| deserialize        | `boolean`  | true  | If true, it will deserialize the documents if they are valid JSON, else they are ignored. |
| deserializeOptions | `struct`   |       | A struct of options to help control how the data is deserialized when populating an object. See ObjectPopulator.cfc for more info. |
| inflateTo          | `any`      |       | A path to a CFC or closure that produces an object to try to inflate the document results on NON-Reduced views only! |
| filter             | `function` |       | A closure or UDF that must return boolean to use to filter out results from the returning array of records, the closure receives a struct that has id, document, key, and value: function( row ). A true will add the row to the final results. |
| transform          | `function` |       | A closure or UDF to use to transform records from the returning array of records, the closure receives a struct that has id, document, key, and value: function( row ). Since the struct is by reference, you do not need to return anything. |
| returnType         | `any`      | array | The type of return for us to return to you. Available options: array, native, iterator  |
| type               | `string`   | view  | The type of query being performed, values are: view or n1ql |
| statement          | `string`   |       | The N1QL statement to be executed |
| parameters         | `any`      |       | Used with N1QL queries, can be a structure or array of parameters for the query |
