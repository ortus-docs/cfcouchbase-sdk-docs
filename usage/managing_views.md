# Managing Views
Views defined via JavaScript a `map` function that populates keys from the data in your bucket. Views can also define an additional `reduce` function that is used to aggregate data down, much like in SQL. Finally, one or more views live inside of a design document, which you can query and construct.

> **Hint** Please read more about views in the Couchbase Docs at http://docs.couchbase.com/couchbase-manual-2.2/#views-and-indexes

You can manage views and design documents from the Couchbase web console and you can also manage them programatically via CFCouchbase as well. Here's a list of some useful methods for view operations:

* `designDocumentExists()` - Check for the existance of a design document.
* `getDesignDocument()` - Retreive a design document and all its views
* `deleteDesignDocument()` - Delete a design document and all its views from the cluster
* `viewExists()` - Check for the existance of a single view
* `saveView()` - Save/update a view and wait for it to index
* `asyncSaveView()` - Save/update a view but don't wait for it to become usable
* `deleteView()` - Delete a single view from its design document

The really nice thing about `saveView()` and `asyncSaveView()` is they either insert or udpate an existing view based on whether it already exists. They also only save to the cluster if the view doesn't exist or is different. This means you can repeatedly call saveView() and nothing will happen on any call but the first. This allows you to specify the views that you need in your application when it starts up and they will only save if neccessary:

```js
// application start
public boolean function onApplicationStart(){
	application.couchbase = new cfcouchbase.CouchbaseClient( { bucketName="beer-sample" } );
	
	// Specify the views the applications needs here.  They will be created/updated
	// when the client is initialized if they don't already exist or are out of date.
	
	application.couchbase.saveView(
		'manager',
		'listBreweries',
		'function (doc, meta) {
		  if ( doc.type == ''brewery'' ) {
		    emit(doc.name, null);
		  }
		}',
		'_count'
	);
			
	application.couchbase.saveView(
		'manager',
		'listBeersByBrewery',
		'function (doc, meta) {
		  if ( doc.type == ''beer'' ) {
		    emit(doc.brewery_id, null);
		  }
		}',
		'_count'
	);
			
	return true;
}
```