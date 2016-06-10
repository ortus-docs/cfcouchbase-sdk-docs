# Installation

## CommandBox Installation (Recommended)

The easiest way to get started with CFCouchbase is to use [CommandBox CLI](https://www.ortussolutions.com/products/commandbox) to install in any directory:

```bash
box install cfcouchbase	
```


## Manual Installation

Download the SDK from the following sources:

* Official Releases: http://www.ortussolutions.com/products/cfcouchbase 
* Source: https://github.com/Ortus-Solutions/cfcouchbase-sdk Github
* Bleeding Edge + More: http://integration.stg.ortussolutions.com/artifacts/ortussolutions/cfcouchbase/


## System Requirements
- Lucee 4.5+
- ColdFusion 10+

## What's Included

- `/cfcouchbase` - This is the actual SDK code
- `/apidocs` - The API docs that show you the FULL list of SDK methods and their arguments (even ones not covered here). There is also an [Online Version] as well.
- `/samples` - A collection of sample apps that use the "beer-sample" bucket.  
    - `/beer-brewery-manager` - Showcases view creation, and basic CRUD operations utilizing native CF data structures
    - `/beer-brewery-manager-oo` - Same as above but with OO design patterns.  Showcases Couchbase CFC inflation
    - `/beer-brewery-manager-mvc` - Same as above but as an MVC application. Requires a /coldbox mapping on your server.
    - `/coldbox-sample` - A sample ColdBox application that shows loading CFCouchbase via a module. Requires a /coldbox mapping. 

> **Hint** The API Docs also have descriptions and code samples for every method. They are a must read as well!   

## The Mapping

The CFCouchase SDK is contained in a single folder.  The easiest way to install it is to copy `cfcouchbase` in the web root.  For a more secure installation, place it outside the web root and create a mapping called `cfcouchbase`.   

```cfml
this.mappings[ '/cfcouchbase' ] = 'C:\path\to\cfcouchbase';
```

Now that the code is in place, all you need to do is create an instance of `cfcouchbase.CouchbaseClient` for each bucket you want to connect to.  The `CouchbaseClient` class is thread safe and you only need one instance per bucket for your entire application.  It is recommended that you store the instantiated client 
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

```cfml
public boolean function onApplicationEnd(){		
	application.couchbase.shutdown( 10 );
	return true;
}
```

> **Danger:** Each Couchbase bucket operates independently and uses its own authentication mechanisms.  You need an instance of `CouchbaseClient` for each bucket you want to interact with. It is also extremely important that you shutdown the clients whenever your application goes down in order to gracefully shutdown connections, else your server might degrade.