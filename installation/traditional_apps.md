# Traditional Apps

Download the SDK from the following sources or use CommandBox: `box install cfcouchbase` to install the SDK

* Official Releases: http://www.ortussolutions.com/products/cfcouchbase 
* Source: https://github.com/Ortus-Solutions/cfcouchbase-sdk Github
* Bleeding Edge + More: http://integration.stg.ortussolutions.com/artifacts/ortussolutions/cfcouchbase/


## The Mapping

The CFCouchase SDK is contained in a single folder.  The easiest way to install it is to copy `cfcouchbase` in the web root.  For a more secure installation, place it outside the web root and create a mapping called `cfcouchbase`.   

```cfml
this.mappings[ '/cfcouchbase' ] = 'C:\path\to\cfcouchbase';
```

Now that the code is in place, all you need to do is create an instance of `cfcouchbase.CouchbaseClient` for each bucket you want to connect to.  The `CouchbaseClient` class is thread safe and you only need one instance per bucket for your entire application.  It is recommended that you store the instantiated client in a persistent scope such as `application` when your app starts up so you can access it easily.

```cfml
public boolean function onApplicationStart(){
    application.couchbase = new cfcouchbase.CouchbaseClient();
    return true;
}
```

## WireBox
However, we would recommend you use a dependency injection framework like [WireBox](https://wirebox.ortusbooks.com) to create and manage your Couchbase Client:

```cfml
// Map our Couchbase Client via WireBox Binder
map( "CouchbaseClient" )
	.to( "cfcouchbase.CouchbaseClient" )
	.initArg( name="config", value={} )
	.asSingleton();
```

## Shutting Down The Client
When you are finished with the client, you need to call its `shutdown()` method to close open connections to the Couchbase server.  The following code sample will wait up to 10 seconds for connections to be closed. 

```cfml
public boolean function onApplicationEnd(){		
	application.couchbase.shutdown( 10 );
	return true;
}
```

> **Danger:** Each Couchbase bucket operates independently and uses its own authentication mechanisms.  You need an instance of `CouchbaseClient` for each bucket you want to interact with. It is also extremely important that you shutdown the clients whenever your application goes down in order to gracefully shutdown connections, else your server might degrade.