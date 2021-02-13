# Retrieving Documents

The easiest way to retrieve a specific document by ID from your Couchbase cluster is by calling the `get()` method.

```javascript
person = client.get( id = "brad" );
```

There are many other methods for getting data. Please check the [API Docs](http://apidocs.ortussolutions.com/cfcouchbase/2.0.0) \(in the download\) to see full descriptions and code samples for all of them.

* `getMulti()` - Get multiple objects from couchbase with a single call.
* `getAndTouch()` - Get a document and update its expiry value
* `getAndLock()` - Get a document and lock it for a specified amount of time
* `getFromReplica()` - Get a document from a replica
* `getWithCAS()` - Get an object from couchbase with a special Compare And Swap \(CAS\) version \(for use wit `replaceWithCAS()`\)
* `n1qlQuery()` - Get a document using a N1QL query

