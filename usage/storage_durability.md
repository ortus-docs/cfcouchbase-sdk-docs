#Storage Durability

Couchbase auto-shards `master` and `replica` documents across your cluster out-of-the-box.  Documents are stored both in RAM for fast access and persisted to disk for long term storage.  By default, all storage operations are `synchronous`, and will return once the document has been written to memory. Which means the `upsert()` call returns potentially before the document is fully stored and replicated.  This is a break from the consistency offered by a typical RDBMS, but it is key to the high-performance and scalable architecture. See the [CAP Theorem](http://en.wikipedia.org/wiki/CAP_theorem).

* If your application requires you to confirm that a document has been persisted to disk, use the `persistTo` argument.  
* If you need to confirm that the document has been copied to a given number replica nodes, use the `replicateTo` argument.

```coldfusion
// This document will be persisted to disk on at least two nodes
client.upsert(
	ID = 'brad',
	value = { name: 'Brad', age: 33, hair: 'red' },
	persistTo = "TWO", 
	replicateTo = "TWO"
);
```

**Note:** All documents will eventually replicate and persist by themselves.  You only need these options if the application cannot continue without it.

There are many other methods for storing data.  Please check the [API docs](http://apidocs.ortussolutions.com/cfcouchbase/2.0.0) to see full descriptions and code samples for all of them.  Here are a few to wet your appetite:

* `upsertMulti()` -  Set multiple documents in the cache with a single operation.
* `replace()` - Replace an existing document.
* `insert()` - Insert a new document
* `replaceWithCAS()` - Update a document only if no one else has changed it since you last retreived it using Compare And Swap (CAS).
* `touch()` - Touch a document to reset its expiration time.
* `unlock()` - Unlock a previously locked document without modifying the document.
* `counter()` -  Increment or Decrement a numeric value
* `prepend()` / `append()` - add content to the beginning or end of an existing document
