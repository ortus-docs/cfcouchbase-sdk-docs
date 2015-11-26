#Data Serialization

Couchbase can literally store anything in a bucket as long as it's represented as a string and no larger than 20MB.  
The CFCouchbase SDK will automatically serialize complex data for you when storing it and deserialize it when you ask for it again.  

###Simple Values 

Before we tell how CFCouchbase serializes your data, we'll tell you how to *avoid* this behavior if you don't want it.  
Simple values (strings) won't be touched, so if you want to control how an array is serialized, just turn it to a **string** first and then pass it into `set()` operations.  These strings can be JSON or anything of your choosing.  When you retrieve these documents back, CFCouchbase will try to deserialize them for you automatically according to our rules you will see soon.  However, if you want the raw data back from Couchbase as a string (regardless of how it was stored),  pass `deserialize=false` into your `get()` or `query()` methods and CFCouchbase will not automatically deserialize it but just pass it back to you.

```coldfusion
// Set my own JSON string
client.set( 
    id="IDidItMyWay", 
    value='{ "title": "My Way", "artist": "Frank Sinatra", "year": "1969"}'
);

// And get it back out as a string
song = client.get( id="IDidItMyWay", deserilize=false );
```

###Complex Data

Complex data (structs, queries, arrays) will be automatically serialized for you with no extra work on your part.  Just pass them into ''set()'' and you'll get the same data structure back from ''get()'' operation.

* **Structs** and **Arrays** will be converted via `serializeJSON()` and stored as JSON so you can query them with views.
* **Queries** - Will be converted to binary with `objectSave()` and wrapped in a struct of metadata containing the *recordcount* and *columnlist*. They are converted to binary so they can be re-inflated back to CFML queries.

```coldfusion
client.set( 
    id='weekDays', 
    value=['Sunday','Monday','Tuesday','Wednesay','Thursday','Friday','Saturday']
);
days = client.get( 'weekDays' );

writeOutput( arrayLen( days ) );
```

###Query Representation

```javascript
{
  "recordcount": 3,
  "type": "cfcouchbase-query",
  "binary": "....",
  "columnlist": "ID,NAME"
}
```