#Custom Transformers

If you don't like how we set up data serialization or just have super-custom requirements, you can provide your own data marshaller to have full control.
Create a CFC that implements the `cfcouchbase.data.IDataMarshaller` interface.  It only needs to have three methods:

* `serializeData()` - Returns the data in a string form so it can be persisted in Couchbase
* `deserializeData()` - Received the raw string data from Couchbase and inflates it as necessary to the original state
* `setCouchbaseClient()` - Gives the marshaller a chance to store a local reference to the client in case it needs to talk back.

```coldfusion
interface{
	any function setCouchbaseClient( required couchcbaseClient );
	any function deserializeData( required string ID, required string data, any inflateTo="", struct deserializeOptions={} );
	string function serializeData( required any data );
}
```

####myDataMarshaller.cfc

```coldfusion
component implements='cfcouchbase.data.IDataMarshaller' {

	any function setCouchbaseClient( required couchcbaseClient ){
		variables.couchbaseClient = arguments.couchcbaseClient;
		return this;
	}

	string function serializeData( required any data ){
		if( !isSimpleValue( data ) ) {
			return serializeJSON( data );
		}
		return data;
	}

	any function deserializeData( required string data, any inflateTo="", struct deserializeOptions={} ){
		if( isJSON( data ) && deserialize ) {
			return deserializeJSON( data );
		}
		return data;
	}
	
}
```

After you have created your custom marshaller, simply pass in an instance of it or the full component path as a config setting:

```coldfusion
// path
couchbase = new cfcouchbase.CouchbaseClient(
	{
		bucketName = 'myBucket',
		dataMarshaller = 'path.to.myDataMarshaller'
	} 
);
// instance
couchbase = new cfcouchbase.CouchbaseClient(
	{
		bucketName = 'myBucket',
		dataMarshaller = new path.to.myDataMarshaller()
	} 
);
```

**Note:** Once you specify a custom data marshaller, you are overriding all **Data Serialization** functionality above.  So you are on your own now buddy!  Like good 'ol spidey says: With Much Power Comes Much Responsibility!