### Installation

Download the SDK from our [http://www.ortussolutions.com/products/cfcouchbase download page] or visit our latest concoctions in [https://github.com/Ortus-Solutions/cfcouchbase-sdk Github], then unzip the file.  

- `/cfcouchbase` - This is the actual SDK code
- `/documentation` - This is a standalone version of the documentation you are reading right now
- `/apidocs` - The API docs that show you the FULL list of SDK methods and their arguments (even ones not covered here). There is also an [Online Version] as well.
- `/samples` - A collection of sample apps that use the "beer-sample" bucket.  
    - `/beer-brewery-manager` - Showcases view creation, and basic CRUD operations utilizing native CF data structures
    - `/beer-brewery-manager-oo` - Same as above but with OO design patterns.  Showcases Couchbase CFC inflation
    - `/beer-brewery-manager-mvc` - Same as above but as an MVC application. Requires a /coldbox mapping on your server.
    - `/coldbox-module-loader` - A sample ColdBox application that shows loading CFCouchbase via a module. Requires a /coldbox mapping. 

**Note:** The API Docs also have descriptions and code samples for every method. They are a must read as well!   

The CFCouchase SDK is contained in a single folder.  The easiest way to install it is to copy `cfcouchbase` in the web root.  For a more secure installation, place it outside the web root and create a mapping called `cfcouchbase`.   

```
this.mappings[ '/cfcouchbase' ] = 'C:\path\to\cfcouchbase';
```

Now that the code is in place, all you need to do is create an instance of `cfcouchbase.CouchbaseClient` for each bucket you want to connect to.  The CouchbaseClient class is thread safe and you only need one instance per bucket for your entire application.  It is recommended that you store the instantiated client 
in a persistent scope such as `application` when your app starts up so you can access it easily.

```cfml
public boolean function onApplicationStart(){
    application.couchbase = new cfcouchbase.CouchbaseClient();
    return true;
}
```

However, we would recommend you use a dependency injection framework like [http://wiki.coldbox.org/wiki/WireBox.cfm WireBox] to create and manage your Couchbase Client:

```cfml
// Map our Couchbase Client via WireBox Binder
map( "CouchbaseClient" )
	.to( "cfcouchbase.CouchbaseClient" )
	.initArg( name="config", value={} )
	.asSingleton();
```

When you are finished with the client, you need to call its `shutdown()` method to close open connections to the Couchbase server.  The following code sample will wait up to 10 seconds for connections to be closed. 

```
public boolean function onApplicationEnd(){		
	application.couchbase.shutdown( 10 );
	return true;
}
```

**Important:** Each Couchbase bucket operates independently and uses its own authentication mechanisms.  You need an instance of `CouchbaseClient` for each bucket you want to interact with. It is also extremely important that you shutdown the clients whenever your application goes down in order to gracefully shutdown connections, else your server might degrade.