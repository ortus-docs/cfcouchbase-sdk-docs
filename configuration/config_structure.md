# Config Structure

The simplest way to get started using the SDK is to simply pass a struct of config settings into the constructor when you create the client.

```cfml
couchbase = new cfcouchbase.CouchbaseClient(
	{
		servers = [
		    "http://cache1:8091", 
		    "http://cache2:8091"
        ],
		bucketName  = "myBucket",
		viewTimeout = 1000
	} 
);
```