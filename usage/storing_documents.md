# Storing Documents

The easiest way to store a document in your Couchbase cluster is by calling the `upsert()` method.  In this example we are passing a struct directly in as the value to be stored.  The SDK will automatically serialize the struct into a JSON document for storage in the cluster.

```coldfusion
client.upsert(
	id = "brad",
	value = { 
	    name: "Brad", 
	    age: 33, 
	    hair: "red", 
	    weird: true 
    } 
);
```

The `id` of the document is `brad` and it will live in the cluster forever until it is deleted.  If I want my document to expire and be automatically removed from the cluster after a certain amount of time, I can specify the `timeout` argument.

This document will be cached for 20 minutes before expiring.  Couchbase will automatically remove it for you once it has expired.

```coldfusion
client.upsert(
	id = "cached-site-menus",
	value = menuHTML,
	timeout = 20
);
```

> **Hint** The CFCouchbase SDK will try to be smart enough to translate any native CFML construct into JSON for you.  You can also pass binary data or JSON as well.  The SDK can also deal with CFC's to provide seamless CFC deserialization and inflation, please see the Data Serialization section.

