# What's New With 2.0.0

More than two years have passed since our SDK v1.1 was released.  The SDK has been updated to support the new functionality available in Couchbase Server 4.x, including:

- N1QL Queries
- GSI Indexes
- Replica Reads
- Document Locking
- Prepared Statements
- Improved Design Document Management
- Expanded Configuration Options
- And more...

## Changes 

CFCouchbase uses the [Java SDK](http://developer.couchbase.com/documentation/server/current/sdks/java-2.2/java-intro.html) from Couchbase, when it went from version 1.x to 2.x breaking changes occurred (for the better).  We have tried to mitigate these changes where possible.  Many methods that were removed from the Java SDK and we created as facades for backwards compatibility.  All of these methods have been marked from deprecation, but are still supported for the time being.

- `add()` => `insert()`
- `decr()` => `counter()`
- `incr()` => `counter()` 
- `delete()` => `remove()` 
- `set()` => `upsert()`
- `setMulti()` => `upsertMulti()`
- `setWithCAS()` > `replaceWithCAS()`

The SDK also brings many new methods, some of which we will cover in detail. 

- `exists()`
- `getAndLock()`
- `getEnvironment()`
- `getFromReplica()`
- `invalidateQueryCache()`
- `n1qlQuery()`
- `publishDesignDocument()`
- `replace()`
- `unlock()`

## N1QL Queries

One of the biggest additions to the SDK is support for [N1QL](http://developer.couchbase.com/documentation/server/current/getting-started/first-n1ql-query.html) (pronounced "nickel").  N1QL is a "SQL like" syntax for working with JSON documents in Couchbase Server.  You can perform a N1QL query by calling the  `query(type="n1ql", statement="...")` method or by calling the `n1qlQuery("...")` method directly.  Just as with View queries, the same options are available to N1QL queries, this includes: 

- `deserialize`
- `deserializeOptions`
- `inflateTo`
- `filter`
- `transform`

The most basic (and fastest) N1QL operation is to retrieve document(s) by their document id.  

```cfml
// query
couchbase.n1qlQuery('
  SELECT u.* 
  FROM users AS u
  USE KEYS "user_101"
');
```

Notice the `FROM` clause, in this example "users" is the name of a bucket.  This query can be described as: 

> "Get every property from the 'user_101' document in the 'users' bucket"

All N1QL queries will return an array, the response from this query would return an array of structures.  

```json
[
  {
    "email": "Judah66@gmail.com",
    "first_name": "Albin",
    "last_name": "Price",
    "user_id": 101,
    "username": "Eudora43"
  }
]
```

The `USE KEYS` statement is not limited to single values either, multiple documents can be retrieved by declaring an array.

```cfml
couchbase.n1qlQuery('
  SELECT u.* 
  FROM users AS u
  USE KEYS [ "user_101", "user_454" ]
');
```

N1QL supports the standard DML operations that you would expect.  

```cfml
// insert a new document
couchbase.n1qlQuery('
  INSERT INTO users (KEY, VALUE)
  VALUES ("user_2343", {
    "user_id": 2343,
    "name": "John Smith",
    "email": "john.smith@gmail.com"
  })
');

// update a specific property of a document
couchbase.n1qlQuery('
  UPDATE users
  USE KEYS "user_2343"
  SET email = "elmariachi@gmail.com"
');

// remove a document
couchbase.n1qlQuery('
  DELETE 
  FROM users
  USE KEYS "user_2343"
');

// update or insert an entire document
couchbase.n1qlQuery('
  UPSERT INTO users (KEY, VALUE)
  VALUES ("user_2343", {
    "user_id": 2343,
    "name": "John Smith",
    "email": "elmariachi@gmail.com"
  })
');
```

Up to this point our queries have used just an ID when working with documents.  What if we wanted to find documents based on a properties value?  To do this we need to create [GSI (Global Secondary) Indexes](http://developer.couchbase.com/documentation/server/current/architecture/global-secondary-indexes.html).

```cfml
// create an index of usernames
couchbase.n1qlQuery('
  CREATE INDEX idx_users_username 
    ON ecommerce( username )
  USING GSI
');

// query based on username
couchbase.n1qlQuery('
  SELECT users.* 
  FROM ecommerce AS users
  WHERE users.username = 'Eudora43'
');
```

There are several [options](http://developer.couchbase.com/documentation/server/current/n1ql/n1ql-language-reference/createindex.html) for creating indexes, such as partitioning, building, covering, array indexing, etc. that are not covered in this post. 


A common practice in SQL is to `JOIN` results across tables.  N1QL supports joining results in a bucket as well as across buckets.  `JOIN` statements utilize the `USE KEYS` clause. 

```cfml
// query to get all of the orders by username
couchbase.n1qlQuery('
  SELECT orders.order_id, orders.order_date, orders.total
  FROM ecommerce AS users
  INNER JOIN ecommerce AS orders 
    ON KEYS users.order_history
  WHERE u.username = 'Eudora43'
');
```

## Document Locking

There may be instances where your application needs to prevent retrieval and updates to a given document.  Document updates can be prevented by using the CAS operations, but this is not always guaranteed.  The 2.0 SDK allows you to retrieve a document and lock it (for up to 30 seconds), then unlock it by updating it with CAS value at a later point in time. 

```cfml
// get the user document and lock it
user = couchbase.getAndLock("user_101");

// update and unlock the document using CAS
couchbase.replaceWithCAS(
  id="user_101",
  value=doc,
  cas=user.cas
);

// unlock the document without updating it
couchbase.unlock(
  id="user_101",
  cas=user.cas
);
```

Calls to retrieve or update document that has been locked, will fail until the document has been successfully unlocked or the maximum time of 30 seconds has passed.

## Config

There are 44 new configuration options supported by the SDK.  The options will allow you to tweak and tune your configuration to meet your needs.  Examples include `caseSensitiveKeys`, `connectTimeout`, `sslEnabled`, `queryTimeout`.  Please see the [documentation](http://developer.couchbase.com/documentation/server/current/sdks/java-2.2/env-config.html#story-h2-2) for detailed descriptions of each configuration option.  

With the changes from the 1.x to 2.x SDK the following configuration options are no longer supported: 

- maxReconnectDelay
- obsPollInterval
- obsPollMax
- opQueueMaxBlockTime
- opTimeout
- readBufferSize
- shouldOptimize
- timeoutExceptionThreshold

## Resources
Please visit our [CFCouchbase](https://www.ortussolutions.com/products/cfcouchbase) page for all the necessary resources.

- [ITB Presentation - CFCouchbase 2.0 + N1QL](https://www.slideshare.net/secret/425CbL6sce5N2b)
- [Couchbase Developer Portal](http://developer.couchbase.com)
- [N1QL Query Tutorial](http://query.pub.couchbase.com/)
- [ColdBox Travel Sample](https://github.com/coldbox-samples/cfcouchbase-travel)
- [N1QL Flight Data Examples](https://github.com/bentonam/fakeit-examples/tree/master/flight-data/docs/n1ql)