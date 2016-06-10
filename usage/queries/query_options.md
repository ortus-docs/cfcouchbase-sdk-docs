# Query Options

Here are some of the most common keys you can pass in the struct of options to control how the query is executed. Please check the [API Docs](http://apidocs.ortussolutions.com/cfcouchbase/2.0.0) for the full list.


| Option | Description |
| -- | -- |
| `sortOrder` 	|	Specifies the direction to sort the results based on the map function's "key" value. Valid values are ASC and DESC.
| `limit` 		|	Number of records to return
| `offset` 		|	Number of records to skip when returning
| `reduce` 		|	Flag to control whether the reduce portion of the view is run. If false, only the results of the map function are returned.
| `includeDocs` |	Specifies whether or not to include the entire document in the results or just the key names. Default is false.
| `startkey` 	|	Specify the start of a range of keys to return.
| `endkey` 		|	Specify the end of a range of keys to return.
| `group` 		|	Flag to control whether the results of the reduce function are grouped.
| `keys` 		|	An array of keys to return. For complex keys, pass each key as an array.
| `stale` 		|	Specifies if stale data can be returned with the view. Possible values are: `OK` - (default)stale data is ok, `FALSE` - force index of view, and `UPDATE_AFTER` potentially returns stale data, but starts an asynch re-index. |
