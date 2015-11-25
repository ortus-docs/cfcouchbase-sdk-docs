##Configuration

The default configuration for CFCouchbase is located in  `/cfcouchbase/config/CouchbaseConfig.cfc`.  You can create the `CouchbaseClient` with no configuration and it will connect to the **default** bucket on [127.0.0.1:8091](http://127.0.0.1:8091).  However, there are more ways to configure the Couchbase client via the `config` argument of the constructor so it can connect to your server and options of choice:

1. Pass a config structure
2. Pass a config CFC

**Note:** The config CFC is the most contained and portable way to store your configuration.

There are a number of configuration options you can set for the client, but most of them can be left at their default value.  To see a full list of options, look in the `/cfcouchbase/config/CouchbaseConfig.cfc` or the user friendly [API Docs](http://apidocs.ortussolutions.com/cfcouchbase/2.0.0).
Here are some of the most common setting you will need to use:

####Common Config Settings

| Setting | Type | Default | Description |
| -- | -- | -- | -- |
| servers           | any     | 127.0.0.1:8091 | The list of servers to connect to. Can be comma-delimited list or an array. If you have more than one server in your cluster, you only need to specify one, but adding more than one will help in the event a node is down when the client connect.  |
| bucketName        | string  | default        | The name of the bucket to connect to on the cluster. This is case-sensitive |
| password          | string  |                | The optional password of the bucket. |
| caseSensitiveKeys | boolean | false          | Keys in Couchbase are case sensitive by default, the SDK by default converts all keys to lowercase, setting this to *true* will prevent this |
| dataMarshaller    | any     |                | The data marshaller to use for serializations and deserializations, please put the class path or the instance of the marshaller to use. Remember that it must implement our interface: `cfcouchbase.data.IDataMarshaller` |

####Config Struct

The simplest way to get started using the SDK is to simply pass a struct of config settings into the constructor when you create the client.

```cfml
couchbase = new cfcouchbase.CouchbaseClient(
	{
		servers = ["http://cache1:8091", "http://cache2:8091"],
		bucketName  = "myBucket",
		viewTimeout = 1000
	} 
);
```

####Config CFC

The most portable method for configuring the client is to use a CFC to place your config settings in much like our other libraries such as [http://wiki.coldbox.org/wiki/WireBox.cfm WireBox] and [http://wiki.coldbox.org/wiki/CacheBox.cfm CacheBox] allow.
To do this simply create a plain CFC with a public method called '''configure()'''.  Inside of that method, put your config settings into the variables scope of the component.  The '''configure()''' method does not need to return any value.  It will be automatically invoked by the SDK prior to the config settings being extracted from the CFC. 

```cfml
component {
	
	function configure() {
		servers = ['http://cache1:8091','http://cache2:8091'];
		bucketName = 'myBucket';
		bucketName = 'myPass';
	}

}
```

To use the config CFC, simply pass it into the CouchbaseClient's constructor.

```cfml
// You can pass an instance
couchbase = new cfcouchbase.CouchbaseClient( new path.to.config() );

// You can also pass a path to the CFC
couchbase = new cfcouchbase.CouchbaseClient( 'path.to.config' );
```