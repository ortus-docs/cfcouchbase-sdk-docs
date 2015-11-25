#Retrieving Documents

The easiest way to retrieve a specific document by ID from your Couchbase cluster is by calling the `get()` method.  

```coldfusion
person = client.get( id = "brad" );
```

There are many other methods for getting data.  Please check the [API Docs](http://apidocs.ortussolutions.com/cfcouchbase/2.0.0) (in the download) to see full descriptions and code samples for all of them.

* `asyncGet()` - Get an object from couchbase asynchronously.
* `getMulti()` - Get multiple objects from couchbase with a single call.
* `getWithCAS()` - Get an object from couchbase with a special Compare And Swap (CAS) version (for use wit `setWithCAS()`)
* `getStats()` -  Get all of the stats from all of the servers in the cluster.
* `getDocStats()` -  Get stats for a specific document ID.