# n1ql Queries

N1QL \(pronounced "nickel"\) queries are a SQL like syntax to query JSON documents in Couchbase Server.

Here are the arguments you can pass into the `query()` or `n1qlQuery()` methods. For the latest and more in-depth information please visit our [API Docs](http://apidocs.ortussolutions.com/cfcouchbase/2.0.0).

| Argument | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| statement | `string` |  | The N1QL statement to be executed |
| parameters | `any` |  | Used with N1QL queries, can be a structure or array of parameters for the query |
| options | `any` | {} | The query options to use for this query. This can be a structure of name-value pairs or an actual Couchbase query options object usually using the `newViewQuery()` method. |
| deserialize | `boolean` | true | If true, it will deserialize the documents if they are valid JSON, else they are ignored. |
| deserializeOptions | `struct` |  | A struct of options to help control how the data is deserialized when populating an object. See ObjectPopulator.cfc for more info. |
| inflateTo | `any` |  | A path to a CFC or closure that produces an object to try to inflate the document results on NON-Reduced views only! |
| filter | `function` |  | A closure or UDF that must return boolean to use to filter out results from the returning array of records, the closure receives a struct that has id, document, key, and value: function\( row \). A true will add the row to the final results. |
| transform | `function` |  | A closure or UDF to use to transform records from the returning array of records, the closure receives a struct that has id, document, key, and value: function\( row \). Since the struct is by reference, you do not need to return anything. |
| returnType | `any` | struct | If returnType is "struct", will return struct containing the results, requestID, signature, and metrics.  If returnType is native, a Java ViewResponse object will be returned \( com.couchbase.client.protocol.views.ViewResponse \)   If returnType is iterator, a Java iterator object will be returned |
| type | `string` | view | The type of query being performed, values are: view or n1ql. \*Note this only required when calling query\(\) to perform a n1ql query |

## Retrieve Document\(s\)

```text
couchbase.n1qlQuery("
SELECT * 
FROM users
USE KEYS "user_101"
");
```

```text
couchbase.n1qlQuery("
SELECT * 
FROM users
USE KEYS ["user_101","user_454"]
");
```

## DML Operations

### Insert

```text
couchbase.n1qlQuery("
INSERT INTO users (KEY, VALUE)
VALUES ("user_2343", {
  "user_id": 2343,
  "name": "John Smith",
  "email": "john.smith@gmail.com"
})
");
```

### Update

```text
couchbase.n1qlQuery("
UPDATE users
USE KEYS "user_2343"
SET email = "john.doe@gmail.com"
RETURNING email
");
```

### Upsert

```text
couchbase.n1qlQuery("
UPSERT INTO users (KEY, VALUE)
VALUES ("user_2343", {
  "user_id": 2343,
  "name": "John Smith",
  "email": "john.smith@gmail.com"
})
RETURNING META().id AS document_id
");
```

### Delete

```text
couchbase.n1qlQuery("
DELETE 
FROM users
USE KEYS "user_2343"
");
```

N1QL queries that do not use a `USE KEYS` statement require the use of [GSI Indexes](http://developer.couchbase.com/documentation/server/current/architecture/global-secondary-indexes.html)

