# Executing Queries

One of the most powerful parts of Couchbase Server is the ability to find data through map / reduce [views and indexes](http://docs.couchbase.com/admin/admin/Views/views-intro.html) or by executing [N1QL \(SQL like\)](http://developer.couchbase.com/documentation/server/4.0/n1ql/index.html) queries which were introduced in Couchbase Server 4.0. We do not cover all the intricacies of views or N1QL, for that please visit the official [Couchbase Docs](http://developer.couchbase.com/documentation/server/4.0/introduction/intro.html). We focus on how to leverage them from CFCouchbase.

There are 2 types of queries available in the SDK:

* [View Queries](view-queries.md)
* [N1QL Queries](n1ql-queries.md)

Both types of queries can be executed through the parent facade of `query()` or through their standalone methods of `viewQuery()` and `n1qlQuery()`.

